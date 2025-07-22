---
date: '2025-07-22T01:32:37-05:00'
title: 'A PostgreSQL Bug I Had to Deal With'
draft: false
cover:
    image: ""
    alt: ""
    relative: false # when usng page bundles set this to true
---

Back in May, when first started working on one of my projects which uses PostgreSQL, I implemented a TypeScript edge function called `delete-user` that does exactly what you'd expect it to do (among other things), and it worked fine back then. But two days ago, to my dismay, I tried deleting an account, and I received a 500 error from the API call.

After adding some logs, I narrowed the error down to this line:

`const { error: deleteError } = await adminClient.auth.admin.deleteUser(user.id);`

What could have changed to make this action fail? My first instinct was a foreign key constraint violation. Essentially, if there exists a table that references the another table, then one must specify that the record in the referencing table should be deleted if the underlying record in the referenced table is deleted. Without specifying that the deletion should cascade, then deleting the underlying record fails. This was entirely plausible because since I had first tested user deletion, I had created many tables.

But it's pretty commonsense to have cascading deletion for user ID, so I doubted that I was missing it. And I was able to confirm that nothing was wrong in that regard.

Then, I dug deeper and found the actual error message from PostgreSQL, which included this: `ERROR: relation \\\"works\\\" does not exist (SQLSTATE 42P01)`. This was confusing at first because my `works` table didn't even reference either my `auth.users` or `profiles` tables. However, it dawned on me that I had two columns used for keeping track of a work's average rating: `rating_count`, the number of total ratings, and `rating_sum`, the sum of all ratings. The way I kept these up-to-date was by using an SQL function that was triggered after updates or deletions on the `book_reviews` table, which did have a `user_id` column.

After trying a lot of things, what ended up being the issue was that in the SQL function, I needed to specify the schema name and write `public.works` since the original deletion was run from the `auth` schema. I excitedly retried deleting the account and was met with another 500, with the specific PostgreSQL error being `ERROR: permission denied for table works (SQLSTATE 42501)`.

This really stumped me because

1. I never see that error during normal application use, probably ruling out RLS policies, and
2. the original edge function was using the service role key, which also bypasses role-related restrictions.

I tried granting permissions to the service role and a few other things, at which point I decided to ask people online. I made posts in two subreddits, which were automatically removed, asked "X" to which noone responded, and asked in a PostgreSQL Discord server which was also fruitless. :cry:

After taking my mind off the issue for about a day, I narrowed down the issue to the following: if the deletion originated with full permissions and cascaded to a process with fewer permissions, then I had to find out which processes could make that possible. Such an explanation should also account for why such a process could normally access the `works` table but could no longer when triggered by a user deletion.

As it turns out, SQL functions have something called `SECURITY DEFINER`, which you can add to them to give them the permissions of the "owner" of the function, which is the `postgres` role and has full access. I guess the default must have been `SECURITY INVOKER` because that was the status of the SQL function I was investigating.

After quickly changing the function to `SECURITY DEFINER`, I was happy to see the delete functionality work!

So why did `SECURITY INVOKER` not work? Right now, the most logical explanation I can think of is that while creating an admin client using the service role key gives basically unrestricted access to the tables through the API, when used to run an SQL function, the context is still `authenticated`, resulting in restricted access (due to the RLS policies in place).

All in all, this whole ordeal was a nice break from writing a lot of code and a nice example of how we think we have fixed a bug and the real problem ends up being something else entirely.
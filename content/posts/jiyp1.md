---
date: '2025-06-30T9:30:29-05:00'
title: 'JIYP: Lessons Learned, Part 1'
draft: false
---

Two weeks ago, I started working at a startup called Jarvis In Your Pocket. Imagine if you could track your nutritional (and overall) health using just an LLM, an LLM that could learn about you, be proactive, and give solid advice based on trusted expert sources selected by your doctor.

Some things were easier than I expected. First, we envisioned a virtual retrieval-augmented generation (RAG) system whereby we could consult with multiple doctors who had their own trusted materials and RAG on a specific subset of all the sources based on which doctor had referred the user. While it may sound like we have to create virtual vector databases on top of the large corpus of content, it's actually as simple filtering on chunk metadata like so:

```
retriever_with_filter = vectorstore.as_retriever(
    search_kwargs={
        "filter": {
            "author": {"$in": ["Alice", "Bob"]}
        }
    }
)
```

Another thing that was easier than expected was doing backend engineering with Replit's Agent. Since we are a small team, we are using Replit for the sake of speed, and we are also using Django because Python allows us to do a lot, and the team has prior experience with it. While I don't know Django that well, Replit's Agent has been wonderful. Whereas in my other projects where I have had to manually maintain and update my PostgreSQL table schema, I have not really had to touch the backend code when I want to make simple backend changes.

On the other end of things, UI has been more challenging than expected. While Replit is good at generating nice-looking UIs (but sometimes breaks already-implemented features), we have still had to put in some thought on how to make the experience frictionless for the user. Thus, we have tried to use Jakob's Law and implement an interface which people are already familiar with, such as existing chats in a left sidebar. But since we are not identical to ChatGPT or other familiar apps, we have still had to come up with new design choices that aim to still be intuitive. And of course, we made sure to incorporate colors, toasts, etc. to make the user more excited and comfortable while using the app.

Finally, it has been really cool to put LangChain to practice with both RAG and agents, especially function calling. One pattern which I think we will find especially useful is allowing our LLM to have access to a bunch of setter functions, each of which updates one piece of information about the user, e.g. `updateFoodPreferences()` or `updateSleepHabits()`. This allows the LLM to maintain an accurate picture of the user's profile in the backend so that they can have a unique tailored experience.
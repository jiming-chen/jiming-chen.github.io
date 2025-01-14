---
date: '2025-01-02T00:19:49-08:00'
draft: false
title: 'OCaml Transformer'
cover:
    image: "/projects/transformer/interface.png"
    alt: "User Interface"
    relative: false # when using page bundles set this to true
hideMeta: true
---

This ({{< newtabref href="https://github.coecis.cornell.edu/hmk68/3110-final-project" title="Cornell GitHub" >}}) was our final project for CS 3110: Data Structures and Functional Programming. We implemented a simple transformer in OCaml and trained it on successful posts on SideChat, a social media app popular at Cornell and other schools. I worked with Haadi Khan, Domenic Fioravanti, and Will Bradley.

{{< youtube qpr12wxoHbg >}}
<figcaption>Fig. 1. YouTube demo of our post generator made by Haadi.</figcaption>


## Probabilistic Model
Before creating the transformer, we first created a probabilistic model.

{{< newimgref src="/projects/transformer/posts.png" alt="GUI" width="80%" >}}
<figcaption>Fig. 2. Examples of posts from SideChat mobile app. These were retrieved by finding a hidden API using a MITM proxy.</figcaption>

The probabilistic model simply kept track of the most likely words to follow any single word, determined by frequency and number of upvotes. To run the model, we started with a beginning word and let it generate a number of words, stopping at a set word count or if an `<END>` token was produced. We also used temperature, so running the model twice produced different results.

With the probabilistic model, words made sense in shorter contexts but not in larger contexts, resulting in incoherent outputs. Here was a coherent output we got:
> Can we all agree with my mom

## Transformer
Next, we implemented the transformer. We followed a basic architecture, implementing a tokenizer, matrix operations, etc. That being said, our we did not use GPUs, so our model size was very limited, and we were not able to decrease loss by that much. However, the outputs were certainly more interesting:
> level football eight-year-old :drooling_face: opinion revert certainly humanities/social WILLING fist ILR,

We only needed 1,600 lines of code for this project, and we also had a time constraint, but in the future, we could use tensor computations from Jane Street's `ocaml-torch` and improve the overall engineering.
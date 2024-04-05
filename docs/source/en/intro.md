# Introduction

## What are leaderboards?

`Leaderboards` are rankings of machine learning artefacts (most frequently generative models, but also embeddings, classifiers, ...) depending on their performance on given tasks across relevant modalities.

They are commonly used to find the best model for a specific use case. 

For example, for Large Language Models, the [Open LLM Leaderboard](https://huggingface.co/spaces/HuggingFaceH4/open_llm_leaderboard) allows you to find the best base pre-trained models in English, using a range of academic evaluations looking at language understanding, general knowledge, and math, and the [Chatbot Arena Leaderboard](https://huggingface.co/spaces/lmsys/chatbot-arena-leaderboard) provides a ranking of the best chat models in English, thanks to user votes on chat capabilities. 

So far on the Hub, we have leaderboards for text, image, video and audio generations, including specialised leaderboard for at least 10 natural (human) languages, and a number of capabilities such as math or code. We also have leaderboards evaluating more general aspects like energy performance or model safety.

Some specific leaderboards reflect human performance obtained through a human-based voting system, where people compare models and vote for the better one on a given task. These spaces are called `arenas`.

## How to use leaderboards properly

There are certain things to keep in mind when using a leaderboard.

1) Comparing apples to apples

Much like in sports, where we have weight categories to keep rankings fair, when evaluating model artefacts, you want to compare similar items.

For example, when comparing models, you want them to be 
- in the same weight class (number of parameters): bigger models often have better performance than smaller ones, but they usually cost more to run and train (in money, time, and energy)
- at the same mathematical precision: the lower the precision of your model, the smaller and faster, but this can affect performance 
- in the same category: pre-trained models are good generalist bases, where fine-tuned models are more specialised and better performing on specific tasks, and merged models tend to have scores higher than their actual performance.

2) Comparing across a spectrum of tasks

Though good generalist machine learning models are becoming increasingly common, it's not because an LLM is good at chess that it will output good poetry. If you want to select the correct model for your use case, you need to look at its scores and performance across a range of leaderboards and tasks, before testing it yourself to make sure it fits your needs.

3) Being careful about evaluation limitations, especially for models

A number of evaluations are very easy to cheat, accidentally or not: if a model has already seen the data used for testing, its performance will be high "artificially", and reflect memorisation rather than any actual capability on the task. This mechanism is called `contamination`. 

Evaluations of closed source models are not always still accurate some time later: as closed source models are behind APIs, it is not possible to know how the model changes and what is added or removed through time (contrary to open source models, where relevant information is available). As such, you should not assume that a static evaluation of a closed source model at time t will still be valid some time later.

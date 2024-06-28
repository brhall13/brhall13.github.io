---
layout: default
title:  "AnythingLLM"
date:   2024-06-15 15:07:10 -0400
categories: ChatGPT Parser
excerpt: While creating any project with a new technology...
tag: LLM
---

# Introduction
While creating any project with a new technology there is a risk of someone else getting to it first. In doing research for my own PDF parser using OpenAI, I came across a program called AnythingLLM (LINK HERE) that handles PDF parsing, chatting, and more. The kicker? It's all run locally using Ollama.

# Ollama
Before I dive into the app, let's review what powers it. [Ollama](https://ollama.com/) is a program that allows you to run different open source LLMs locally, such as Llama 3, Mistral, or Gemma. As well as the promising new class of SLMs (Small Language Models) such as Phi 3. By running locally you can make sure the data is kept private, utilize the model that makes most sense for the task, and keep API costs to a minimum. 

There is one problem though, you are limited based on your compute power. So for instance my Macbook Air (which is my main coding machine) struggles to run more complex models while handling other tasks. To work around this, you can use AnythingLLM and some simple port forwarding to have Ollama run through your network on a more powerful PC, in this case my Windows gaming desktop.

![Ollama](/images/ollama.png)

# Anything LLM
[Anything LLM](https://useanything.com/) is an open source, all-in-one AI desktop and Docker app. It is capable of ingesting documents like PDF and TXT, creating embeddings, managing vector databases, RAG, and more traditional chat completion. One of the most recent features (requiring an API key) is AI Agents, allowing your LLM to browse the web and run code on your behalf.


In my testing with document parsing and web scraping, I noticed the app struggling with documents that had too many large images breaking up formatting. Stripping the text from the pages and into a .rtf or .txt file helped mitigate this, however this may change in the future as multimodal LLMs become more widely available. 

![AnythingLLM](/images/anythingllm.png)

Using this, I was able to create a chatbot grounded in the knowledge of my custom data completely private and local. There are a few additional things to pay attention to regarding inputting data. The token cutoff and buffer amounts can make a big difference in the viability of RAG, and whether the model knows if two subjects are connected. 

I'm very excited by the prospect of the local LLM and SLM scene exploding in growth as the compute power and training sizes become smaller and smaller to reflect more niche use cases. 
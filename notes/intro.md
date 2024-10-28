# Introduction to LLM

This notes focus on the high-level concepts of large language model (LLM), especially:

- What is a large language models, focusing on the next word prediction problem.
- Transformer, no technique details are involved here.
- Sampling problem, how to choose a word based on the probability given by the model.
- Training and Finetuning, the basic flow of creating an LLM.

## What is a (Large) Language Model?

**Next Word Prediction**

A language model outputs a **probability distribution** over the next token in a sequence given the previous tokens. Simply put, a language model predicts the next word or token (considering a token as a word for now).

Predicting the next word is crucial because **many practical NLP tasks can be cast as word prediction**. For example, in text summarization, you can feed the language model with the full-length article followed by a "tl;dr" (too long; didn't read) token. The language model then predicts the next tokens, which form the summary of the article.

**N-gram Model**

Historically, language models were statistical **n-gram models**. These models approximated the sequence history by looking back at **a few words** (n-words) rather than the full history. In contrast, today's LLMs with **transformer** architectures can consider the entire sequence history, not just a few words.

**Large**

"Large" refers to both the model’s size in terms of parameters and the immense dataset on which it’s trained.

## Transformer

Details about the transformer technique are not covered here.

**LLM vs Transformer**

Currently, LLM and transformer are used interchangeably because most LLMs are based on the transformer architecture. However, not all transformers are LLMs since transformers can also be used for computer vision, and not all LLMs are transformers, as some are based on recurrent and convolutional architectures.

**Encoder & Decoder**

Transformers (introduced in the paper "Attention is All You Need") were originally developed for machine translation, such as translating English texts to German and French. They consist of two parts: an **encoder** and a **decoder**.

- The encoder processes the input text and encodes it into a series of numerical representations or vectors that capture the contextual information of the input.
- The decoder takes these encoded vectors and generates the output text.

Both the encoder and decoder consist of many layers connected by a **self-attention** mechanism, which allows the model to weigh the importance of different words or tokens in a sequence relative to each other. This enables the model to capture long-range dependencies and contextual relationships within the input data, enhancing its ability to generate coherent and contextually relevant output.

GPT focuses on the decoder submodule, while BERT focuses on the encoder side.

## Sampling for LLM Generation

Based on the context and probabilities given by the model, how do you choose a word?

- **Greedy Decoding**: Always choose the token with the maximum probability. This method is predictable and deterministic but can be boring.
- **Random Sampling**: Randomly choose a word. This method doesn't work well because it often selects odd words with low probability.
- **Top-K Sampling**: A generalization of greedy decoding. Instead of choosing the most probable word, randomly choose among the top k most probable words.
- **Nucleus or Top-P Sampling**: Instead of choosing a fixed number of top k probable words, choose among the words that make up the top p percent of the probability distribution.
- **Temperature Sampling**: Instead of truncating the distribution like top-k or top-p, reshape the distribution by dividing the probabilities by a temperature parameter.

All sampling methods must balance **quality** and **diversity**. Methods emphasizing the most probable words tend to produce outputs rated as more accurate, coherent, and factual but also more predictable. Methods giving more weight to middle-probability words tend to be more creative and diverse.

## Training and Finetuning

**Self-supervised Training**

Self-supervised training, the model generates its own lable from is used to training LLMs. When training the model, we ask it to predict the next word, the correct word is on the original corpus. 

**Teacher Focing**

Notes that when we move to next word, we ignore what the model predicted for the previous word, instead use the correct sequence to estimate the next word.

**Pretraining & Finetuning**

The general process of creating an LLM includes pretraining and fine-tuning. The “pre” in “pretraining” refers to the initial phase where a model like an LLM is trained on a large, diverse dataset to develop a broad understanding of language. This pretrained model then serves as a foundational resource that can be further refined through **fine-tuning**, a process where the model is specifically trained on a narrower dataset that is more specific to particular tasks or domains

**Fine-tuning Methods**

- **Continued Pre-training**: Retrain all the parameters of the model on new, domain-specific data using the same methods and loss functions as in pretraining. This is akin to appending the new data to the tail of the pretraining data.
- **Parameter-efficient Fine-tuning (PEFT)**: Instead of retraining all parameters (which is slow and expensive), PEFT selectively updates specific parameters during fine-tuning, leaving the rest at their pretrained values.
- **Feature-based Fine-tuning**: Commonly used with masked language models like BERT. In this method, the entire pretrained model is frozen, and only the classification head is trained on new, labeled data for specific tasks.
- **Supervised Fine-tuning (SFT)**: Often used for instruction fine-tuning, where the goal is to train a pretrained language model to follow text instructions, such as answering questions or following commands.

## Evaluating LLM 

todo: how to evaluate an LLM ?

## Scale

Todo: **Scaling Law** and **KV Cache**

## Hallucination

**Hallucination**

Language models are designed to generate text that is predictable and coherent. However, the training algorithms do not have mechanisms to ensure that the generated text is correct or true.

**RAG**

The key idea of RAG (Retrieval-Augmented Generation) is to use information retrieval (IR) techniques to fetch documents that are likely to contain relevant information for answering a question. Then, a large language model generates an answer based on these retrieved documents.

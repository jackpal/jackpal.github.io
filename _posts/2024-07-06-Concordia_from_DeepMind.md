---
date: 2024-07-06 13:00:00
description: A LLM agent framework.
tags: ML, AI, DeepMind, LLM, Gemma_2, Mixtral, Agents
title: Concordia from DeepMind
---

I tried out the [Concordia](https://github.com/google-deepmind/concordia) LLM-driven agent framework.

I ran it on my own macbook, using [ollama](https://ollama.com/) to serve a variety of LLMs. I used [Visual Studio Code](https://code.visualstudio.com/) to run the Concordia example notebooks. I used venv to manage the Python virtual environment.

To adapt the notebook to run Ollama, I first used `ollama pull <model-name>` to install each model locally, then I changed

```python
from concordia.language_model import gpt_model

model = gpt_model.GptLanguageModel(api_key=GPT_API_KEY,
                                   model_name=GPT_MODEL_NAME)
```

to

```python
from concordia.language_model import ollama_model

model = ollama_model.OllamaLanguageModel(model_name=OLLAMA_MODEL_NAME)
```

I tried gemma2:9b, gemma2:27b, and mixtral:8x7b.

gemma2:9b worked, but had trouble adhering to the prompts.

gemma2:27b is currently broken in Ollama 0.1.48, it doesn't stay on topic. Supposedly will be fixed in 0.1.50.

Mixtral ran too slowly to be useful.

I didn't get a good result, but I think if I kept trying with different models I might find a good result.

Overall the concept seems promising. It's kind of the authors to make their toolkit available!

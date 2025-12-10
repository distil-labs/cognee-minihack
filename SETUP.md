# Setting Up Question Answering with a Local Model

This guide explains how to set up question answering using the cognee-distillabs local model with Ollama.

## Prerequisites

### 1. Install Ollama

Install Ollama from [ollama.com/](https://ollama.com/) or using your package manager.

### 3. Register Ollama Model

Navigate to the models directory containing the downloaded model files and the register the models with:

```bash
cd models

ollama create nomic-embed-text -f nomic-embed-text/Modelfile
ollama create cognee-distillabs-model-gguf-quantized -f cognee-distillabs-model-gguf-quantized/Modelfile
```

### 4. Graph Setup

You will need a virtual environment (venv) configured for this, so if you didn't create one beforehand, here is how you
can do it:

```bash
# You can easily do if with the help of uv
uv venv

# In case you don't have uv installed, you can do the following command
python -m venv venv

# After this, you can activate the environment
source .venv/bin/activate
```

In order to import the generated graph into your venv memory, you should run the following script:

```bash
python setup.py
```

## 5. Configuration

Modify the `solution_q_and_a.py` script by adding the following environment variable configuration **at the top of the file** (note, it won't work unless the addition starts at line 1 of the new file):

```python
import os

# Since we are using Ollama locally, we do not need an API key, although it is important that it is defined, and not an empty string.
os.environ["LLM_API_KEY"] = "."
os.environ["LLM_PROVIDER"] = "ollama"
os.environ["LLM_MODEL"] = "cognee-distillabs-model-gguf-quantized"
os.environ["LLM_ENDPOINT"] = "http://localhost:11434/v1"
os.environ["LLM_MAX_TOKENS"] = "16384"

os.environ["EMBEDDING_PROVIDER"] = "ollama"
os.environ["EMBEDDING_MODEL"] = "nomic-embed-text:latest"
os.environ["EMBEDDING_ENDPOINT"] = "http://localhost:11434/api/embed"
os.environ["EMBEDDING_DIMENSIONS"] = "768"
os.environ["HUGGINGFACE_TOKENIZER"] = "nomic-ai/nomic-embed-text-v1.5"
```

## 6. Running the Script

Once configured, run the question answering script:

```bash
python solution_q_and_a.py
```

## 7. Example Results

Example results comparing LLM and SLM outputs can be found in `responses.txt`.


## Appendix: Using base QWEN4 model with ollama

To use the base QWEN3 model, Navigate to the models directory containing the downloaded model files and the register the models with:

```bash
cd models

ollama create Qwen3-4B-Q4_K_M -f Qwen3-4B-Q4_K_M/Modelfile
```

Once its loaded, you can use it using standard [OpenAI interface](https://ollama.com/blog/openai-compatibility). For exampel from python as

```python
from openai import OpenAI

client = OpenAI(
    base_url = 'http://localhost:11434/v1',
    api_key='ollama', # required, but unused
)

response = client.chat.completions.create(
  model="Qwen3-4B-Q4_K_M",
  messages=[
    {"role": "system", "content": "You are a helpful assistant."},
    {"role": "user", "content": "Who won the world series in 2020?"},
    {"role": "assistant", "content": "The LA Dodgers won in 2020."},
    {"role": "user", "content": "Where was it played?"}
  ]
)
print(response.choices[0].message.content)
```
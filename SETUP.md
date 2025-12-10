# Setting Up Question Answering with a Local Model

This guide explains how to set up question answering using the cognee-distillabs local model with Ollama.

## Prerequisites

### 1. Install Ollama

Install Ollama from [ollama.com/](https://ollama.com/) or using your package manager.

### 2. Download the Model

Create a new directory and download the model from HuggingFace:

```bash
mkdir -p models && cd models
```

**Option A: Using huggingface-cli (recommended)**

```bash
# Install the Hugging Face CLI if not already installed
pip install huggingface_hub

# Download the model repository
huggingface-cli download distillabs/cognee-distillabs-gguf-quantized --local-dir cognee-distillabs-gguf-quantized

# If the command above does not work, you can try the following command
hf download distillabs/cognee-distillabs-gguf-quantized --local-dir cognee-distillabs-gguf-quantized
```

**Option B: Using Git LFS**

```bash
# Install git-lfs if not already installed
# macOS: brew install git-lfs
# Ubuntu: apt install git-lfs

git lfs install
git clone https://huggingface.co/distillabs/cognee-distillabs-gguf-quantized
```

### 3. Create the Ollama Model

Navigate to the directory containing the downloaded model files and run:

```bash
ollama create cognee-distillabs-model-gguf-quantized
ollama pull nomic-embed-text
```

### 4. Graph Setup

You will need a virtual environment (venv) configured for this, so if you didn't create one beforehand, here is how you can do it:
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

Modify the `solution_q_and_a.py` script by adding the following environment variable configuration at the top of the file:

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

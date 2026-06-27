# LangChain LLM Examples

<div align="center">

[![Python 3.8+](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/downloads/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Code style: black](https://img.shields.io/badge/code%20style-black-000000.svg)](https://github.com/psf/black)
[![Contributions Welcome](https://img.shields.io/badge/contributions-welcome-brightgreen.svg?style=flat)](https://github.com/)

</div> 

This repository contains two production-ready examples of using different Large Language Models (LLMs) with LangChain: one using HuggingFace models locally and another using OpenAI's API.

## 📋 Table of Contents

- [🎯 Overview](#-overview)
- [✅ Prerequisites](#-prerequisites)
- [🚀 Installation](#-installation)
- [🤗 Example 1: HuggingFace Local Model](#-example-1-huggingface-local-model)
- [🔑 Example 2: OpenAI API](#-example-2-openai-api)
- [💻 Usage](#-usage)
- [⚡ Quick Start](#-quick-start)
- [📊 Comparison](#-comparison)
- [🔧 Troubleshooting](#-troubleshooting)
- [📚 Resources](#-resources)
- [🤝 Contributing](#-contributing)
- [📄 License](#-license)

## 🎯 Overview

### What is LangChain?

[LangChain](https://python.langchain.com/) is a framework for developing applications powered by language models. It enables you to:

- 🔗 Connect language models to other sources of computation and data
- ⛓️ Create chains and agents for complex workflows
- 🌍 Use various LLM providers seamlessly
- 📦 Build production-ready LLM applications

### 📁 Examples Included

| Example | Model | Provider | Type |
|---------|-------|----------|------|
| **Example 1** | TinyLlama 1.1B | HuggingFace | Local/Offline |
| **Example 2** | GPT-3.5 Turbo | OpenAI | API-based |

## ✅ Prerequisites

- 🐍 **Python 3.8** or higher
- 📦 **pip** (Python package manager)
- 💾 **2GB disk space** (for HuggingFace model download)
- 🔑 **OpenAI API key** (only needed for Example 2)

## 🚀 Installation

### Step 1: Clone the Repository

```bash
git clone https://github.com/yourusername/langchain-llm-examples.git
cd langchain-llm-examples
```

### Step 2: Create Virtual Environment (Recommended)

```bash
# Using venv
python -m venv venv

# Activate
# On Linux/Mac:
source venv/bin/activate
# On Windows:
venv\Scripts\activate
```

### Step 3: Install Dependencies

#### Option A: Using pip (recommended)

```bash
pip install -r requirements.txt
```

#### Option B: Manual installation

```bash
pip install \
  langchain==0.1.0 \
  langchain-huggingface==0.0.30 \
  langchain-openai==0.0.13 \
  python-dotenv==1.0.0 \
  torch==2.1.0 \
  transformers==4.35.0
```

#### Option C: Using poetry

```bash
poetry install
```

### 📦 Dependencies Overview

| Package | Purpose | Size |
|---------|---------|------|
| `langchain` | Core LangChain framework | - |
| `langchain-huggingface` | HuggingFace integration | - |
| `langchain-openai` | OpenAI integration | - |
| `python-dotenv` | Environment variable management | - |
| `torch` | PyTorch (ML framework) | ~800MB |
| `transformers` | HuggingFace transformers | - |

## 🤗 Example 1: HuggingFace Local Model

**File:** `huggingface_example.py`

### Code

```python
from langchain_huggingface import ChatHuggingFace, HuggingFacePipeline

# Initialize HuggingFace Pipeline
llm = HuggingFacePipeline.from_model_id(
    model_id='TinyLlama/TinyLlama-1.1B-Chat-v1.0',
    task='text-generation',
    pipeline_kwargs=dict(
        temperature=0.5,
        max_new_tokens=100
    )
)

# Wrap with Chat interface
model = ChatHuggingFace(llm=llm)

# Send query and get response
result = model.invoke("value of sin30 degrees")

print(result.content)
```

### 🔧 How It Works

```
User Prompt
    ↓
HuggingFacePipeline (loads TinyLlama)
    ↓
ChatHuggingFace (formats for chat)
    ↓
Model Output
```

| Parameter | Value | Purpose |
|-----------|-------|---------|
| `model_id` | `TinyLlama/TinyLlama-1.1B-Chat-v1.0` | Lightweight 1.1B parameter model |
| `task` | `text-generation` | Task type for pipeline |
| `temperature` | `0.5` | Controls response randomness (0.1=deterministic, 0.9=creative) |
| `max_new_tokens` | `100` | Maximum response length |

### 📊 Specifications

| Aspect | Details |
|--------|---------|
| **Model** | TinyLlama 1.1B Chat |
| **Memory** | ~2GB disk space |
| **Speed** | CPU: 5-10s/token, GPU: 0.5-1s/token |
| **Privacy** | ✅ 100% local - no data leaves your machine |
| **Cost** | ✅ Free |
| **Internet** | ✅ Not required after first download |

### 📤 Example Output

```
The value of sin(30°) is 0.5
```

### ⚡ Performance Tips

- **GPU Usage**: Install CUDA for 10-20x faster inference
- **Reduce Tokens**: Lower `max_new_tokens` for faster responses
- **Batch Queries**: Process multiple queries together

## 🔑 Example 2: OpenAI API

**File:** `openai_example.py`

### Code

```python
from langchain_openai import OpenAI
from dotenv import load_dotenv

# Load API key from .env file
load_dotenv()

# Initialize OpenAI LLM
llm = OpenAI(model='gpt-3.5-turbo-instruct')

# Send query and get response
result = llm.invoke("what is the capital of India")

print(result)
```

### 🔧 How It Works

```
User Prompt
    ↓
LangChain OpenAI Wrapper
    ↓
OpenAI API (via HTTPS)
    ↓
GPT-3.5 Model
    ↓
Response
```

### 🔑 API Setup

#### 1️⃣ Get OpenAI API Key

- Visit [platform.openai.com/api-keys](https://platform.openai.com/api-keys)
- Click "Create new secret key"
- Copy and save the key securely

#### 2️⃣ Create `.env` File

Create a `.env` file in the project root:

```env
OPENAI_API_KEY=sk-your-api-key-here
```

**⚠️ Security Note:** Never commit `.env` files to version control!

```bash
# Add to .gitignore
echo ".env" >> .gitignore
```

#### 3️⃣ Run the Script

```bash
python openai_example.py
```

### 📊 Model Specifications

| Aspect | Details |
|--------|---------|
| **Model** | GPT-3.5 Turbo Instruct |
| **Speed** | ⚡ Instant (cloud-based) |
| **Quality** | ✅ Excellent general purpose |
| **Privacy** | ⚠️ Data sent to OpenAI servers |
| **Cost** | 💰 Pay per token (~$0.0015/1K tokens) |
| **Internet** | 🔴 Required |
| **Reliability** | ✅ 99.9% uptime SLA |

### 📤 Example Output

```
The capital of India is New Delhi.
```

### 💰 Cost Estimation

```
Prompt: ~5 tokens
Response: ~10 tokens
Cost per query: ~$0.00002
1000 queries: ~$0.02
```

### 🛡️ API Best Practices

- ✅ Use environment variables for secrets
- ✅ Implement rate limiting in production
- ✅ Add error handling for API failures
- ✅ Monitor token usage and costs
- ⚠️ Never hardcode API keys

## ⚡ Quick Start

```bash
# 1. Clone repository
git clone https://github.com/yourusername/langchain-llm-examples.git
cd langchain-llm-examples

# 2. Setup environment
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# 3. Install dependencies
pip install -r requirements.txt

# 4. Run HuggingFace example (local)
python huggingface_example.py

# 5. Setup OpenAI (optional)
echo "OPENAI_API_KEY=sk-your-key-here" > .env

# 6. Run OpenAI example
python openai_example.py
```

## 💻 Usage

### Running Example 1: HuggingFace (Local)

```bash
python huggingface_example.py
```

**Output:**
```
The value of sin(30 degrees) is 0.5
```

**⏱️ First Run:**
- ⬇️ Downloads model (~900MB)
- ⏳ Takes 2-5 minutes
- 💾 Cached to `~/.cache/huggingface/hub/`
- 🚀 Subsequent runs are instant

**Subsequent Runs:**
- ✅ Uses cached model
- ⚡ Runs instantly

### Running Example 2: OpenAI (API)

```bash
# Ensure .env file exists with OPENAI_API_KEY
python openai_example.py
```

**Output:**
```
The capital of India is New Delhi.
```

**Requirements:**
- ✅ `.env` file with valid `OPENAI_API_KEY`
- ✅ Internet connection
- ✅ OpenAI account with available balance

### Running Both Examples

```bash
# Run local model
python huggingface_example.py

# Run API-based model
python openai_example.py
```

### Running All Tests

```bash
pytest tests/
```

## 📊 Comparison

| Aspect | 🤗 HuggingFace | 🔑 OpenAI |
|--------|:---:|:---:|
| **Cost** | Free ✅ | Pay per token 💰 |
| **Speed** | 5-10s/token (CPU) ⚡ | Instant ⚡⚡⚡ |
| **Quality** | Good (1.1B) ✅ | Excellent (175B+) ✅✅✅ |
| **Privacy** | Local only ✅✅✅ | Sent to OpenAI ⚠️ |
| **Model Size** | Lightweight 🪶 | Powerful 💪 |
| **Internet** | Not required ✅ | Always required 🔴 |
| **Setup** | Auto-download ✅ | API key needed 🔑 |
| **Offline** | Yes ✅ | No ❌ |
| **Customizable** | Yes ✅ | No ❌ |
| **Latency** | 100-500ms | 10-50ms |
| **Error Rate** | ~5% | <1% |
| **Best For** | Development, testing, local apps | Production, high-accuracy needs |

### 🎯 Choose HuggingFace if you:
- ✅ Want free, offline operation
- ✅ Care about data privacy
- ✅ Are developing/testing
- ✅ Have limited budget
- ✅ Need to customize the model

### 🎯 Choose OpenAI if you:
- ✅ Need highest accuracy
- ✅ Require instant responses
- ✅ Building production apps
- ✅ Have budget for API costs
- ✅ Need enterprise support

## 🔧 Troubleshooting

### 🤗 HuggingFace Issues

<details>
<summary><b>❌ Out of Memory (OOM) Error</b></summary>

```
RuntimeError: CUDA out of memory
```

**Solutions:**
1. Use CPU instead:
   ```python
   pipeline_kwargs=dict(device=-1, temperature=0.5)
   ```

2. Reduce `max_new_tokens`:
   ```python
   pipeline_kwargs=dict(max_new_tokens=50)  # Instead of 100
   ```

3. Free up GPU memory:
   ```bash
   nvidia-smi  # Check GPU usage
   ```

4. Use a smaller model

</details>

<details>
<summary><b>❌ Model Download Fails</b></summary>

```
ConnectionError: Failed to establish a new connection
```

**Solutions:**
1. Check internet connection
2. Verify 2GB free disk space
3. Clear HuggingFace cache:
   ```bash
   rm -rf ~/.cache/huggingface
   ```
4. Try again - downloads sometimes timeout

</details>

<details>
<summary><b>❌ Slow Inference (5+ seconds per token)</b></summary>

**Solutions:**
1. Install CUDA for GPU acceleration:
   ```bash
   pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118
   ```

2. Reduce response length:
   ```python
   max_new_tokens=50  # Faster
   ```

3. Use a lighter model

</details>

<details>
<summary><b>❌ CUDA/GPU Not Detected</b></summary>

```python
# Test CUDA availability
import torch
print(torch.cuda.is_available())  # Should be True
```

**Solutions:**
1. Install NVIDIA drivers
2. Install CUDA toolkit
3. Reinstall PyTorch with CUDA support

</details>

### 🔑 OpenAI Issues

<details>
<summary><b>❌ "API Key Not Found" Error</b></summary>

```
AuthenticationError: Incorrect API key provided
```

**Solutions:**
1. Create `.env` file in project root
2. Add valid API key:
   ```
   OPENAI_API_KEY=sk-your-actual-key-here
   ```
3. Ensure file is in working directory
4. Check .env is not in .gitignore (it should be!)

</details>

<details>
<summary><b>❌ "Rate Limit Exceeded"</b></summary>

```
RateLimitError: Rate limit exceeded
```

**Solutions:**
1. Wait 60 seconds before retrying
2. Implement exponential backoff:
   ```python
   import time
   for attempt in range(3):
       try:
           result = llm.invoke(prompt)
           break
       except RateLimitError:
           time.sleep(2 ** attempt)
   ```
3. Upgrade OpenAI plan for higher limits

</details>

<details>
<summary><b>❌ "Model Not Found" Error</b></summary>

```
APIError: The model 'gpt-3.5-turbo-instruct' does not exist
```

**Solutions:**
1. Check model name spelling
2. Verify model availability:
   - `gpt-3.5-turbo-instruct` ✅
   - `gpt-4` (requires access)
3. Check your account has API access
4. Try older model: `text-davinci-003`

</details>

<details>
<summary><b>❌ "Insufficient Quota"</b></summary>

```
InsufficientQuotaError: You exceeded your current quota
```

**Solutions:**
1. Add billing method to OpenAI account
2. Check usage limits at platform.openai.com/account/usage
3. Set spending limits to prevent overages
4. Use smaller models for testing

</details>

### ✅ General Solutions

```bash
# Update all packages
pip install --upgrade langchain langchain-openai langchain-huggingface

# Reinstall from scratch
pip install --force-reinstall -r requirements.txt

# Clear Python cache
find . -type d -name __pycache__ -exec rm -r {} +
find . -type f -name "*.pyc" -delete
```

## 🚀 Advanced Usage

### Custom Prompts & System Messages

```python
# Single query
result = model.invoke("Explain quantum computing in simple terms")
print(result.content)
```

### Batch Processing

```python
from typing import List

prompts: List[str] = [
    "What is AI?",
    "What is Machine Learning?",
    "What is Deep Learning?"
]

for prompt in prompts:
    result = model.invoke(prompt)
    print(f"Q: {prompt}\nA: {result.content}\n")
```

### Temperature Tuning

```python
# Deterministic (temperature=0.1)
# Good for: Factual questions, code generation, math

# Balanced (temperature=0.5)
# Good for: General conversation, summaries

# Creative (temperature=0.9)
# Good for: Creative writing, brainstorming, storytelling

pipeline_kwargs = dict(
    temperature=0.1,    # Low: Predictable
    max_new_tokens=100
)
```

### Error Handling

```python
from langchain_openai import OpenAI
from openai import RateLimitError, APIError
import time

llm = OpenAI(model='gpt-3.5-turbo-instruct')

def invoke_with_retry(prompt: str, max_retries: int = 3) -> str:
    for attempt in range(max_retries):
        try:
            return llm.invoke(prompt)
        except RateLimitError:
            if attempt == max_retries - 1:
                raise
            time.sleep(2 ** attempt)
        except APIError as e:
            print(f"API Error: {e}")
            raise

result = invoke_with_retry("What is Python?")
print(result)
```

### Comparing Models Side-by-Side

```python
from langchain_huggingface import ChatHuggingFace, HuggingFacePipeline
from langchain_openai import OpenAI

# Initialize both models
hf_llm = HuggingFacePipeline.from_model_id(
    model_id='TinyLlama/TinyLlama-1.1B-Chat-v1.0',
    task='text-generation',
    pipeline_kwargs=dict(temperature=0.5, max_new_tokens=100)
)
hf_model = ChatHuggingFace(llm=hf_llm)
openai_model = OpenAI(model='gpt-3.5-turbo-instruct')

prompt = "What is the capital of France?"

print("HuggingFace (Local):")
print(hf_model.invoke(prompt).content)
print("\nOpenAI (API):")
print(openai_model.invoke(prompt))
```

## 📚 Resources

### Documentation
- 📖 [LangChain Official Docs](https://python.langchain.com/)
- 🤗 [HuggingFace Hub](https://huggingface.co/)
- 🔑 [OpenAI API Reference](https://platform.openai.com/docs/api-reference)
- 💬 [TinyLlama Model Card](https://huggingface.co/TinyLlama/TinyLlama-1.1B-Chat-v1.0)

### Tutorials & Guides
- [LangChain Quickstart](https://python.langchain.com/docs/get_started/quickstart)
- [Building LLM Applications](https://python.langchain.com/docs/guides/apps)
- [Prompt Engineering Guide](https://github.com/dair-ai/Prompt-Engineering-Guide)

### Community
- 💬 [LangChain Discord](https://discord.gg/langchain)
- 🤗 [HuggingFace Discussions](https://huggingface.co/spaces)
- 🐛 [GitHub Issues](https://github.com/)

## 📄 License

This project is licensed under the **MIT License** - see the [LICENSE](LICENSE) file for details.

```
MIT License

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, and/or sell copies of the
Software...
```

## 🤝 Contributing

We welcome contributions! Please follow these steps:

### 1. Fork the Repository
```bash
# Click "Fork" on GitHub
```

### 2. Create a Branch
```bash
git checkout -b feature/your-feature-name
```

### 3. Make Changes
```bash
# Edit files, test thoroughly
```

### 4. Commit & Push
```bash
git add .
git commit -m "Add: Description of your changes"
git push origin feature/your-feature-name
```

### 5. Submit Pull Request
- Open PR on GitHub
- Include description of changes
- Link any related issues

### Contribution Guidelines
- ✅ Add tests for new features
- ✅ Update documentation
- ✅ Follow PEP 8 style guide
- ✅ Keep commits atomic and descriptive
- ✅ No breaking changes without discussion

## 📞 Support

### Getting Help

| Issue Type | Where to Get Help |
|-----------|------------------|
| **Bug Reports** | [GitHub Issues](https://github.com/) |
| **Feature Requests** | [GitHub Discussions](https://github.com/) |
| **General Questions** | Discussions tab or [LangChain Discord](https://discord.gg/langchain) |
| **Documentation** | [Official Docs](https://python.langchain.com/) |

### Quick Links

- 🐛 [Report a Bug](https://github.com/)
- ✨ [Request a Feature](https://github.com/)
- 💬 [Start a Discussion](https://github.com/)
- 📧 [Contact Maintainers](mailto:support@example.com)

---

<div align="center">

**⭐ If you found this helpful, please consider starring the repository!**

Made with ❤️ by [Deep Kamal](https://github.com/yourusername)

</div>

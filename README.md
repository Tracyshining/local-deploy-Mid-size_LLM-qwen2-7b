```markdown
# local-deploy-Mid-size_LLM-qwen2-7b

A step-by-step, beginner-friendly guide to locally deploy and run free mid-size LLMs, using Qwen2-7B as an example. This guide is specifically tailored for deployment on an NVIDIA RTX 3090 .More information will be coming soon.



**Prerequisites:**

  System: Ubuntu 20.04/22.04 LTS (64-bit)



  Hardware: RTX 3090 (24GB VRAM, compatible with other NVIDIA GPUs)



  Base Environment: NVIDIA drivers + CUDA 11.8+ installed (verifiable via `nvidia-smi`)



**Install Ollama:**

```bash

curl -fsSL https://ollama.com/install.sh | sh

sudo curl -fsSL https://ollama.com/install.sh | sh

```

if that doesn't work, try the second command above; if it still fails, try update software sources and install necessary dependencies.



**Verify installation:**

```bash

ollama --version

```



**Run Qwen2-7B**

```bash

ollama pull qwen2:7b

ollama run qwen2:7b

ollama serve

```



The first run will automatically download the model (~13GB).

Ollama provides a simple HTTP API that can be invoked using the requests library，After downloading，install the requests library:

```bash

pip install requests

```

```

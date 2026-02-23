local-deploy-Mid-size_LLM-qwen2-7b 完整部署指南

# local-deploy-Mid-size_LLM-qwen2-7b

A step-by-step, beginner-friendly guide to locally deploy and run free mid-size LLMs, using Qwen2-7B as an example. This guide is specifically tailored for deployment on an NVIDIA RTX 3090 (24GB VRAM), with all steps, commands and notes integrated completely, no separation.

## Prerequisites

- System: Ubuntu 20.04/22.04 LTS (64-bit), with ≥20GB free disk space (for model files and dependencies) and a stable internet connection (to download the ~13GB model file).

- Hardware: NVIDIA RTX 3090 (24GB VRAM); compatible alternatives include other NVIDIA GPUs with ≥16GB VRAM (e.g., RTX 4090, A100); CPU ≥8-core, system RAM ≥16GB (minimum requirement).

- Base Environment: NVIDIA GPU drivers (≥525.xx version) + CUDA ≥11.8 (CUDA 12.x recommended for better compatibility) installed. You can verify the environment with the following commands:

nvidia-smi  # Check GPU status and CUDA version displayed
nvcc -V     # Verify CUDA compiler version (supplementary check)

If CUDA is not installed, follow NVIDIA's official guide for Ubuntu: https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html

## Step 1: Install Ollama

Ollama is a lightweight tool for managing and running LLMs locally, with built-in support for Qwen2-7B and automatic GPU acceleration. Note: Do NOT run the install script with `sudo` unless absolutely necessary (may cause permission issues); use the standard command first.

### Installation Commands

1. Standard installation (recommended):

curl -fsSL https://ollama.com/install.sh | sh

2. If the standard installation fails (due to permission or network issues):

# Update system packages first
sudo apt update && sudo apt install -y curl wget
# Re-run installation with sudo
sudo curl -fsSL https://ollama.com/install.sh | sh

### Verify Installation

After installation, run the following commands to confirm Ollama is installed successfully and the service is running:

ollama --version  # Should output Ollama version (e.g., ollama version 0.1.48+)
systemctl status ollama  # Check if Ollama service is running (Ubuntu/Debian)

If the Ollama service is not running, execute the following command to start and enable it (ensures Ollama starts automatically on system boot):

sudo systemctl start ollama && sudo systemctl enable ollama

## Step 2: Deploy and Run Qwen2-7B

### Step 2.1: Download the Qwen2-7B Model

Run the following command to pull (download) the Qwen2-7B model (≈13GB); the download time depends on your network speed (usually 10-30 minutes for average broadband). The RTX 3090’s 24GB VRAM is sufficient for the full 7B model, so no quantization is needed:

ollama pull qwen2:7b

### Step 2.2: Run Qwen2-7B Interactively (Terminal Chat)

After the model is downloaded completely, start an interactive chat session with the model in the terminal using the following command:

ollama run qwen2:7b

Example usage:
>>> Explain machine learning in simple terms
Machine learning is a branch of artificial intelligence that lets computers learn from data without being explicitly programmed for every task...

>>> To exit the chat session: type /bye and press Enter

### Step 2.3: Run Ollama as a Background Service (API Access)

If you want to use Qwen2-7B via HTTP API (instead of the terminal), run Ollama as a background service with the following command:

ollama serve

By default, the API runs on http://localhost:11434. You can invoke the API using the requests library; first, install the requests library with the following command:

pip install requests

### Example of API Call (Using Requests Library)

After installing the requests library, you can use the following Python code to call the Qwen2-7B model via API:

import requests

def call_qwen2_7b(prompt):
    url = "http://localhost:11434/api/generate"
    payload = {
        "model": "qwen2:7b",
        "prompt": prompt,
        "stream": False  # Set to True if you want streaming responses
    }
    response = requests.post(url, json=payload)
    if response.status_code == 200:
        return response.json().get("response", "No response from model.")
    else:
        return f"API request failed with status code: {response.status_code}"

# Test the API call
if __name__ == "__main__":
    user_prompt = "Explain the difference between AI and machine learning in simple terms."
    result = call_qwen2_7b(user_prompt)
    print("Model Response:", result)

## Common Issues and Solutions

1. Installation fails due to network issues: Check your internet connection, or replace the install script source with a mirror (if available); alternatively, download the install script manually and run it.

2. Model download is slow or interrupted: Re-run `ollama pull qwen2:7b`; Ollama supports resumable downloads.

3. GPU is not used (model runs on CPU): Verify that NVIDIA drivers and CUDA are installed correctly (run `nvidia-smi` to confirm), and ensure Ollama is using the GPU (it will be automatically detected if the environment is configured properly).

4. Permission errors when running Ollama: Reinstall Ollama with `sudo` (as shown in the installation steps) or adjust file permissions with `sudo chmod -R 755 /usr/local/bin/ollama`.

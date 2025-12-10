# Self-Hosted Private AI: Run Locally on Your Laptop or PC, on Your Home Server (Proxmox VE), or in the Cloud on a Virtual Private Server (VPS).

Welcome to your ultimate guide for **self-hosting a fully private AI** on your own computer, home server, or in the cloud!  
Whether you’re on **Windows**, **Linux**, a **home server with Proxmox VE**, or a **VPS**, this guide will walk you **step by step** through installing, configuring, and running your AI locally or remotely.

---

We will cover how to **self-host [Open WebUI](https://github.com/open-webui/open-webui)** using [**Ollama**](https://github.com/ollama/ollama) as the LLM runner.

Open WebUI is an **extensible, feature-rich, and user-friendly AI platform** designed to operate entirely offline.
Ollama allows you to run LLMs locally with a simple interface, and Open WebUI provides a **built-in inference engine for RAG (retrieval-augmented generation)** — making it a powerful solution for deploying AI privately and securely.

**Additionally**, we will show you how to **expose your AI safely for external access**, depending on the deployment method you choose, using:

- **Netbird** – secure private networking.
- **Nginx with Let’s Encrypt SSL** – encrypted web access.  
- **Cloudflare Tunnels** – controlled remote connections.

---

## Why Self-Host Your AI?

- **🔒 Full privacy:** Keep all data on your machine, home server, or VPS.  
- **🧪 Experiment safely:** Test AI without cloud restrictions.  
- **⚙️ Full customization:** Choose models, runners, and inference workflows.  
- **💰 Save costs:** Run locally or on a VPS without paying expensive cloud fees.  
- **🚀 Scalability:** Deploy on a laptop, home server, or remote VPS depending on your needs.   

**Hardware Requirements:**
  
> **(CPU):** A modern multi-core CPU with at least 4 cores is recommended. Higher clock speeds and multiple cores improve AI performance, especially when running models without a GPU.
  
> **(RAM):** For a smooth AI experience, we recommend a minimum of 16 GB of RAM for one user. More RAM (32 GB+) allows you to run larger models or multiple processes simultaneously without slowdowns. Less than 16 GB may limit model sizes or performance.

> **(GPU):** A dedicated GPU is strongly recommended for AI tasks, particularly for model training or running large models. NVIDIA GPUs are the most commonly supported; AMD GPUs can also work with some frameworks. Before proceeding, ensure your GPU drivers are up-to-date by following the official driver guides from your hardware manufacturer. A supported GPU can drastically improve speed and responsiveness.

---

## <img width="29" height="33" alt="linux" src="https://github.com/user-attachments/assets/014fd61d-a933-4112-90fa-ea30141fcb44" /> For Linux Users

You can skip the WSL installation and proceed directly.

---
## <img width="29" height="33" alt="windows" src="https://github.com/user-attachments/assets/13a9c536-b927-4759-9a5c-526d24ed4808" /> For Windows Users

We recommend installing the **Windows Subsystem for Linux (WSL)**, which allows you to run a full Linux environment directly on Windows.

**Open the Command Prompt (cmd) and run:**



    wsl --install


This will:  

- Enable WSL on your Windows system.  
- Install the latest WSL 2 version.

Once installation completes, **restart your computer**.  
After reboot, you can check available Linux distributions by opening the Command Prompt (cmd) and running:


    wsl --list --online


This will display a list of valid Linux distributions you can install with WSL.


<img width="1916" height="965" alt="Screenshot From 2025-12-10 08-32-18" src="https://github.com/user-attachments/assets/0b6a6b16-9ce4-429d-9f00-b27708f5bbc2" />


To install your preferred distribution (e.g., Debian, Ubuntu), run:

    wsl --install -d Debian


<img width="1916" height="965" alt="Screenshot From 2025-12-10 08-42-41" src="https://github.com/user-attachments/assets/a8bd3ff3-7bc9-4b18-b0ed-16ae40de993a" />


After this installation, **restart your computer again**.  

Once rebooted, launch the Linux distro from the Start Menu and proceed with the setup.


---

## 🎯 Next Steps

Follow the commands below carefully — they will:  

- Install necessary dependencies  
- Set up your local AI environment  
- Prepare your system to run AI models privately  

Let’s get your **private AI running locally**!

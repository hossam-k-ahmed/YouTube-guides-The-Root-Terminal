# Self-Hosted Your AI: Run Locally on Your Laptop or PC, Home Server (Proxmox VE), or in the Cloud on a Virtual Private Server (VPS).

Whether you’re on **Windows**, **Linux**, a **home server with Proxmox VE**, or a **VPS**, this guide will walk you **step by step** through installing, configuring, and running your AI locally or remotely.

I will cover how to **self-host [Open WebUI](https://github.com/open-webui/open-webui)** using [**Ollama**](https://github.com/ollama/ollama) as the LLM runner.

Open WebUI is an **extensible, feature-rich, and user-friendly AI platform** designed to operate entirely offline.

Ollama allows you to run LLMs locally with a simple interface, and Open WebUI provides a **built-in inference engine for RAG (retrieval-augmented generation)** — making it a powerful solution for deploying AI privately and securely.

**Additionally**, i will show you how to **expose your AI safely for external access**, depending on the deployment method you choose, using:

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
  
> **(CPU):** A modern multi-core CPU with at least 6~8 cores is recommended. Higher clock speeds and multiple cores improve AI performance, especially when running models without a GPU.
  
> **(RAM):** For a smooth AI experience, i recommend a minimum of 16 GB of RAM per user. More RAM (32 GB+) allows you to run larger models or multiple processes simultaneously without slowdowns. Less than 16 GB may limit model sizes or performance.

> **(GPU):** A dedicated GPU is strongly recommended for AI tasks, particularly for model training or running large models. NVIDIA GPUs are the most commonly supported, AMD GPUs can also work with some frameworks. Before proceeding, ensure your GPU drivers are up-to-date by following the official driver guides from your hardware manufacturer. A supported GPU can drastically improve speed and responsiveness.

---

## <img width="29" height="33" alt="linux" src="https://github.com/user-attachments/assets/014fd61d-a933-4112-90fa-ea30141fcb44" /> Linux Users & Home Servers (Proxmox VE)

This guide works on **any modern Linux distribution**.

Compatible systems include popular distros such as **Debian**, **Ubuntu**, **Fedora** and **Arch Linux**.

[**Please proceed to the Firewall Configuration**](https://github.com/hossam-k-ahmed/YouTube-Guides-The-Root-Terminal/blob/main/How%20to%20Run%20Your%20Own%20AI%20Locally%20(100%25%20Private%20+%20Easy%20Setup)/Instructions.md#1-firewall-configuration)  

> **Tip:** Make sure your system is fully updated to avoid dependency issues.

---
## <img width="29" height="33" alt="windows" src="https://github.com/user-attachments/assets/13a9c536-b927-4759-9a5c-526d24ed4808" /> Windows Users

I recommend installing the **Windows Subsystem for Linux (WSL)**, which allows you to run a full Linux environment directly on Windows.

**Open the Command Prompt (cmd) and run:**



    wsl --install


This will:  

- Enable WSL on your Windows system.  
- Install the latest WSL 2 version.

**⚠️ Important:** Once installation completes, **restart your computer**.  
After reboot, you can check available Linux distributions by opening the Command Prompt (cmd) and running:


    wsl --list --online


This will display a list of valid Linux distributions you can install with WSL.


<img width="1916" height="965" alt="Screenshot From 2025-12-10 08-32-18" src="https://github.com/user-attachments/assets/0b6a6b16-9ce4-429d-9f00-b27708f5bbc2" />


To install your preferred distribution (e.g., Debian, Ubuntu), run:

    wsl --install Debian


<img width="1916" height="965" alt="Screenshot From 2025-12-10 08-42-41" src="https://github.com/user-attachments/assets/a8bd3ff3-7bc9-4b18-b0ed-16ae40de993a" />


**⚠️ Important:** After this installation, restart your computer again.
Once rebooted, launch your Linux distro from the Start Menu to continue setup.


---
## 1. Firewall Configuration:

Before exposing any services, it's important to configure a secure firewall.  
For **Debian-based systems (Debian/Ubuntu)** and **Arch Linux**, we will use **UFW (Uncomplicated Firewall)** because it is simple, reliable, and easy to configure.  
For **Fedora**, we will use **firewalld**, the default and recommended firewall management tool on RPM-based systems.


#  <img width="29" height="33" alt="debian" src="https://github.com/user-attachments/assets/1665750e-83dc-4dc2-af42-169cd577a9dc" /> Debian & Ubuntu (UFW)
### 1. Update, Upgrade, and Remove Unused Packages.

    sudo apt update && sudo apt dist-upgrade -y && sudo apt autoremove -y

### 2. Install UFW (if missing).
    sudo apt install ufw -y

### 3. Set Secure Defaults.
    sudo ufw default allow outgoing
    sudo ufw default deny incoming

### 4. Allow SSH (with rate limiting).
    sudo ufw limit 22/tcp

### 5. Allow Ollama API (11434).
    sudo ufw allow 11434/tcp

### 6. Allow Open WebUI Port (3000).
    sudo ufw allow 3000/tcp

### 7. Enable UFW.
    sudo ufw enable
    sudo systemctl enable ufw
    sudo ufw status verbose


# <img width="29" height="33" alt="arch" src="https://github.com/user-attachments/assets/861319ac-38af-4b4f-badb-3fb1980fcd65" /> Arch Linux (UFW)
### 1. Update The System.

    sudo pacman -Syu

### 2. Install UFW (if missing).
    sudo pacman -S ufw

### 3. Set Secure Defaults.
    sudo ufw default allow outgoing
    sudo ufw default deny incoming

### 4. Allow SSH (with rate limiting).
    sudo ufw limit 22/tcp

### 5. Allow Ollama API (11434).
    sudo ufw allow 11434/tcp

### 6. Allow Open WebUI Port (3000).
    sudo ufw allow 3000/tcp

### 7. Enable UFW.
    sudo ufw enable
    sudo systemctl enable ufw
    sudo ufw status verbose


# <img width="29" height="33" alt="fedora" src="https://github.com/user-attachments/assets/497b8709-eb66-4e1f-8a35-7c48cc79d314" /> Fedora (firewalld)
### 1. Update The System.
    sudo dnf upgrade --refresh -y

### 2. Allow SSH.
    sudo firewall-cmd --permanent --add-service=ssh

### 3. Allow Ollama API Port (11434).
    sudo firewall-cmd --permanent --add-port=11434/tcp

### 4. Allow Open WebUI Port (3000).
    sudo firewall-cmd --permanent --add-port=3000/tcp

### 5. Apply The Changes.
    sudo firewall-cmd --reload
    sudo firewall-cmd --list-all

### 6. Enable firewalld.
    sudo dnf install firewalld -y
    sudo systemctl enable --now firewalld


## 2. Install Ollama on Linux:

Installing **Ollama** is very simple and works on most modern Linux distributions, including **Debian, Ubuntu, Arch, and Fedora**.

### Run the Official Installer
Execute the following command in your terminal.
⚠️ Important: Always verify scripts downloaded from the internet. This is the official Ollama installation script.

    curl -fsSL https://ollama.com/install.sh | sh

### Verify the Installation
After installation, check that Ollama is correctly installed, You should see the installed version printed in your terminal.
    
    ollama --version

### Verify the API in the Browser
On your local machine open the browser and go to:
    
    http://localhost:11434
If running on a VPS:

    http://Your-IP-Address:11434
You should see a confirmation or API status page, indicating the Ollama API is running correctly.

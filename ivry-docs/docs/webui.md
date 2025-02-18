# **Ivry WebUI Guide**

Ivry provides a **Gradio-based WebUI** that allows users to interact with models visually. This guide explains how to launch, configure, and troubleshoot the WebUI on **Windows** and **macOS/Ubuntu**.

---

## **WebUI on Windows**

### 1. Unlocking features and install wsl2

#### 1.1 use install bat to install wsl2
Open wsl_install.bat as an administrator.

- Right click -> Run as administrator...

You should see: 


![Output from running the above commands successfully.](assets/images/wsl_install.png)

If you cannot run this file successfully, try:

Go to microsoft store and install [ubuntu-22.04](https://apps.microsoft.com/detail/9pn20msr04dw?ocid=webpdpshare)

#### 1.2 reboot
Before moving forward, make sure you reboot your computer so that Windows 11 will have WSL2 and virtualization available to it.

### 2. Init wsl2 environment

#### 2.1 install requirements

Install requirements by 

```bash
. install.sh
```

note: if your wsl cannot do apt update, you can open install.sh in notebook and copy paste each steps

### 3. Run webui

Run webui by:
```bash
python ui.py
```


## **WebUI on macOS / Ubuntu**

### 1. Install Cloudflared

Install Cloudflared to ensure your network safety

macOS:
```bash
brew install cloudflare/cloudflare/cloudflared
```

Ubuntu:
```bash
wget https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-amd64.deb
dpkg -i cloudflared-linux-amd64.deb
```


### 2. Clone the Ivry repository

```bash
git clone https://github.com/neural-image/ivry.git
```

### 3. Install the CLI

```bash
cd ivry_cli
. install.sh
```

### 4. Run webui

Run webui by:
```bash
python ui.py
```

## **Troubleshots**

If you encounter websocket problem, you could try to reinstall websocket by:
```bash
pip uninstall websockets
pip install websockets
```
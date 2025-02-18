# **Ivry CLI Documentation**

## **1. Introduction**

### **What is Ivry?**
Ivry is a command-line interface (CLI) tool designed for managing AI workflows, deploying models, and automating cloud-based inference services. It provides an intuitive and efficient way to initialize AI projects, manage cloud deployments, and interact with frameworks like ComfyUI.

Ivry supports **both command-line and web-based interfaces**, making it easy to manage AI models through a terminal or a graphical WebUI.

---

## **Key Features**
Ivry provides a wide range of functionalities to streamline AI model execution and deployment:

### **1. AI Project Initialization**
- **Quickly set up a new project** with `ivry init_app <project_name>`.
- Uses pre-configured templates to simplify AI model development.

### **2. Web-based User Interface (WebUI)**
- **Built-in WebUI** (`ui.py`) for managing workflows visually.
- Allows **real-time model monitoring and execution** through a browser.
- Runs locally but can be accessed remotely if needed.

### **3. AI Model Deployment**
- **Upload models to the cloud** with `ivry upload_app`.
- **Update existing models** using `ivry update_app` without downtime.
- Store and retrieve model credentials securely.

### **4. Server Management**
- **Start a model server** with `ivry start_server`.
- **Stop active servers** using `ivry stop_server`.
- Run AI inference as a service.

### **5. WebSocket Communication**
- Integrates with **ComfyUI** for interactive AI workflows.
- Real-time data exchange via WebSocket (`websocket_comfyui.py`).

### **6. Cloud Automation**
- **Cloudflare API integration** to automate deployments.
- **Remote AI inference** using `tunnel_config.yaml`.

### **7. CLI Utility Commands**
- Retrieve API keys, manage credentials, and parse model configurations.

---

## **Use Cases**
Ivry is designed for **AI engineers, researchers, and automation developers** who need a streamlined CLI tool for:

✅ **Initializing AI projects with minimal setup.**  
✅ **Managing AI models via a WebUI or CLI interface.**  
✅ **Deploying AI models to the cloud for production inference.**  
✅ **Automating AI workflows with real-time WebSocket integration.**  

---
## **Next Steps**
Now that you have an overview of Ivry’s capabilities, let’s move on to the **Installation** section, where we’ll cover system requirements, dependencies, and how to get started.

🚀 **Next:** 

[Ubuntu Installation Guide](getting-started/linux.md) →

[macOS Installation Guide](getting-started/mac.md) →

[Windows Installation Guide](getting-started/windows.md) →
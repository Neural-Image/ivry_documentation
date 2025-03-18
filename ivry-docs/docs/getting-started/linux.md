# **Linux Installation Guide**

## **Step 1: Install ivry_cli**

**1. Clone the ivry_cli repository**

```bash
git clone https://github.com/Neural-Image/ivry_cli.git -b comfyUI
```

**2. Install the CLI**

```bash
cd ivry_cli
```

```bash
./install.sh
```

if you encounter a permission error:
```bash
chmod +x install.sh
./install.sh
```

you can check your installation by
```bash
ivry_cli
```

It will show a simple introduction to the cli, you could use "q" to quit

**4. Authenticate using an API key**

You need to go to our website and become a developer to get the apikey [ivry website](https://ivry.co/account)

And then, login from ivry cli:
```bash
ivry login --auth_token YOUR_API_KEY
```

---

**5. Initializing a Project**

Currently, the CLI supports two modes: `comfyui` and `model`.

```bash
project-x init_app --project_name {project_name} --mode {comfyui/model}
```

### Examples:
- For **ComfyUI**:
  ```bash
  project-x init_app --project_name my_project --mode comfyui
  ```
- For **Model-based projects**:
  ```bash
  project-x init_app --project_name my_project --mode model
  ```

Once initialized, a project folder is created, and a `predict.py` file will be available. Edit `predict.py` according to your model or workflow requirements.

---

**6. Uploading Your Project**

### Important: Use Absolute Paths in `predict.py`

Your `predict.py` file is located under:
```bash
/ivry_cli/{project_name}/predict.py
```
Modify the script as needed, following the provided comments, and then upload your app:

```bash
project-x upload_app --model_name {project_name}
```
Or navigate to the project directory and execute:
```bash
cd {project_name}
project-x upload_app
```

---

**7. Managing Your Model**

### Check Uploaded Models
```bash
project-x list_models
```

### Update an Existing Model
If you update `predict.py` after uploading, you can update your model:
```bash
project-x update_app --model_id {model_id} --model_name {project_name}
```
Or use:
```bash
cd {project_name}
project-x update_app --model_id {model_id}
```

---

**8. Hosting Your Project**

### Start the Server

```bash
cd {project_name}
project-x start_server
```

### Stop the Server
```bash
project-x stop_server
```

---

**9. Troubleshooting**

### WebSocket Issues
If you encounter WebSocket errors when starting the server, try:
```bash
pip uninstall websockets
pip install websocket-client
```
# **Macos Installation Guide**

## **1. Clone the ivry_cli repository**

 ```bash
 git clone https://github.com/Neural-Image/ivry_cli.git -b comfyUI
 ```

---
## **2. Install the CLI**

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

```bash
source ~/.zshrc
```


you can check your installation by
```bash
ivry_cli
```

It will show a simple introduction to the cli, you could use "q" to quit

---

## **3. Authenticate using an API key**

You need to go to our website and become a developer to get the apikey [ivry website](https://ivry.co/account)

And then, login from ivry cli:
```bash
ivry login YOUR_API_KEY
```

---

## **4. Create Your App**

 we support comfyUI apps and python apps.

### **For ComfyUI creator:**

1. Create your app on [ivry website](https://www.ivry.co/login)

2. Pull your project to cli (windows users):

    When using ComfyUI with Ivry CLI, ensure:

    a. ComfyUI is running with the `--listen` flag to enable API access

    b. Port 8188 (or your configured port) is accessible

      ```bash
      ivry_cli pull_project --app_id your_app_id # you can specify your port by adding --comfyui_port
      ```

    Example:

      ```bash
      ivry_cli pull_project --app_id 123 
      ```

     You can see your project locate at: /ivry_project/comfyUI_project/app_{your_app_id}/

### **For Python creator:**

1. Create your predict.py 

    You could find a template in /src/templates/predict.py

    After you finished it, please save it to the root of the repo:
    ```
    --comfyui_workflow
    --docs
    --src
    -predict.py
    -READEME.md
    ```

2. Generate predict_signature.json 
   
      ```bash
      ivry_cli parse_predict
      ```
   
     That will generate a predict_signature.json file in the same directory, we will use it later

3. Create your app on [ivry website](https://www.ivry.co/login)

4. Pull your project to cli (windows users):

      ```bash
      ivry_cli pull_project --app_id your_app_id
      ```

    Example:

      ```bash
      ivry_cli pull_project --app_id 123
      ```

---

## **5. Host Your App**

Start both the ivry_cli model server and cloudflared tunnel:

 ```bash
 cd path/to/your/app
 ivry_cli run_server --force
 ```

### Specify a project path

 Please make sure your current path is at the root directory of the cli

 ```bash
 ivry_cli run_server --project project_folder_name --force #like app_30
 ```

### Stopping the Server

 ```bash
 # Stop all running ivry services
 ivry_cli stop_server [--project_path PATH] [--force]
 ```

The `--force` option allows you to terminate services that may be stuck or not responding to normal shutdown commands.

---

## **6. Troubleshooting**

### WebSocket Issues
If you encounter WebSocket errors when starting the server, try:
```bash
pip uninstall websockets
pip install websocket-client
```
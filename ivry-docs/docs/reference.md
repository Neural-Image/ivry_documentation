# CLI Reference

This page provides detailed information about all available Ivry CLI commands, their options, and usage examples.

## Authentication Commands

### Login

Authenticate with the Ivry platform using your API key.

```bash
ivry_cli login --auth_token YOUR_API_KEY
```

**Parameters:**

- `--auth_token` (required): Your Ivry API key, which can be obtained from the Ivry website

**Example:**

```bash
ivry_cli login --auth_token a1b2c3d4e5f6g7h8i9j0
```

**Response:**

```
Token saved in /home/user/.ivry/token.txt
```

## Project Management Commands

### List Apps

List all your applications registered on the Ivry platform.

```bash
ivry_cli list_apps
```

**Example:**

```bash
ivry_cli list_apps
```

**Response:**

```
Applications retrieved successfully!

================================================================================
ID              Name                      isPublic         state           Created Date
--------------------------------------------------------------------------------
123             stable_diffusion_app      True             active          2025-03-01
456             image_segmentation        False            inactive        2025-02-15
================================================================================
Retrieved 2 applications.
```

### Pull Project

Pull an existing project from the Ivry platform to your local environment.

```bash
ivry_cli pull_project --app_id APP_ID [--comfyui_port PORT] [--project_name NAME] [--comfyUI_dir DIR]
```

**Parameters:**

- `--app_id` (required): ID of the application to pull
- `--comfyui_port` (optional): Port of the ComfyUI process (default: `8188`)
- `--project_name` (optional): Custom name for the local project directory
- `--comfyUI_dir` (optional): Path to your ComfyUI installation

**Example:**

```bash
ivry_cli pull_project --app_id 123 --comfyUI_dir /home/user/ComfyUI
```

**Response:**

```
Getting 123's config...
folder ivry_project/comfyUI_project/app_123 created
app_config.json saved to ivry_project/comfyUI_project/app_123
tunnel_config.json saved to ivry_project/comfyUI_project/app_123
app 123 pulled to app_123/ folder
```

### Parse Predict File

Parse a predict.py file to generate a predict_signature.json file.

```bash
ivry_cli parse_predict [--predict_filename FILE]
```

**Parameters:**

- `--predict_filename` (optional): Path to the predict.py file (default: `predict.py`)

**Example:**

```bash
ivry_cli parse_predict --predict_filename custom_predict.py
```

**Response:**

```
Created predict_signature.json
```

## Server Management Commands

### Run Server

Start the Ivry server and Cloudflare tunnel using PM2 process manager.

```bash
ivry_cli run_server [--project PROJECT] [--force]
```

**Parameters:**

- `--project` (optional): Path to the project directory (default: current directory)
- `--force` (optional): Force restart services even if they're already running

**Example:**

```bash
ivry_cli run_server --project app_123 --force
```

**Response:**

```
Starting ivry_cli model server and cloudflared tunnel for project at: ivry_project/comfyUI_project/app_123
Model ID: 123abc456def789
Logs will be written to: ivry_project/comfyUI_project/app_123/logs
Log files will be limited to 1MB each
Services started with PM2.
To view status: ivry_cli pm2_status
To control services: ivry_cli pm2_control [start|stop|restart]
To view logs: ivry_cli pm2_logs
To stop all services: ivry_cli stop_server
```

### Stop Server

Stop all Ivry services managed by PM2.

```bash
ivry_cli stop_server [--project PROJECT] [--force]
```

**Parameters:**

- `--project` (optional): Path to the project directory (default: current directory)
- `--force` (optional): Forcibly stop all processes

**Example:**

```bash
ivry_cli stop_server --force
```

**Response:**

```
All ivry services have been forcibly terminated.
```

### Start Server Components (Legacy)

Start individual server components (use `run_server` instead for most cases).

```bash
ivry_cli start {model|business} [--upload-url URL] [OPTIONS]
```

**Parameters:**

- `model|business`: Component to start
- `--upload-url` (optional): URL for file uploads
- Additional options passed to the server

**Example:**

```bash
ivry_cli start model --upload-url=https://www.ivry.co/pc/client-api/upload
```

## PM2 Management Commands

### PM2 Status

Display the status of Ivry services managed by PM2.

```bash
ivry_cli pm2_status [--project PROJECT]
```

**Parameters:**

- `--project` (optional): Path to the project directory (default: current directory)

**Example:**

```bash
ivry_cli pm2_status
```

**Response:**

```
PM2 process status:

┌────────────────────┬────┬──────┬───────┬────────┬─────────┬────────┬────────┬──────┬───────────┬──────────┬──────────┐
│ App name           │ id │ mode │ pid   │ status │ restart │ uptime │ cpu    │ mem  │ user      │ watching │ version  │
├────────────────────┼────┼──────┼───────┼────────┼─────────┼────────┼────────┼──────┼───────────┼──────────┼──────────┤
│ ivry_server        │ 0  │ fork │ 12345 │ online │ 0       │ 2h     │ 0.5%   │ 56MB │ user      │ disabled │ 0.1.0    │
│ ivry_cloudflared_… │ 1  │ fork │ 12346 │ online │ 0       │ 2h     │ 0.2%   │ 30MB │ user      │ disabled │ N/A      │
└────────────────────┴────┴──────┴───────┴────────┴─────────┴────────┴────────┴──────┴───────────┴──────────┴──────────┘
```

### PM2 Control

Control PM2 processes for Ivry services.

```bash
ivry_cli pm2_control COMMAND [--process PROCESS] [--project PROJECT]
```

**Parameters:**

- `COMMAND` (required): Command to execute ('start', 'stop', 'restart', or 'reload')
- `--process` (optional): Process to control ('ivry_server', 'ivry_cloudflared_tunnel', or 'all') (default: 'all')
- `--project` (optional): Path to the project directory (default: current directory)

**Example:**

```bash
ivry_cli pm2_control restart --process ivry_server
```

**Response:**

```
command 'pm2 restart ivry_server' success 

status:
┌────────────────────┬────┬──────┬───────┬────────┬─────────┬────────┬────────┬──────┬───────────┬──────────┬──────────┐
│ App name           │ id │ mode │ pid   │ status │ restart │ uptime │ cpu    │ mem  │ user      │ watching │ version  │
├────────────────────┼────┼──────┼───────┼────────┼─────────┼────────┼────────┼──────┼───────────┼──────────┼──────────┤
│ ivry_server        │ 0  │ fork │ 12347 │ online │ 1       │ 0s     │ 0.5%   │ 56MB │ user      │ disabled │ 0.1.0    │
│ ivry_cloudflared_… │ 1  │ fork │ 12346 │ online │ 0       │ 2h     │ 0.2%   │ 30MB │ user      │ disabled │ N/A      │
└────────────────────┴────┴──────┴───────┴────────┴─────────┴────────┴────────┴──────┴───────────┴──────────┴──────────┘
```

### PM2 Logs

View logs from PM2-managed Ivry services.

```bash
ivry_cli pm2_logs [--process PROCESS] [--lines LINES] [--project PROJECT]
```

**Parameters:**

- `--process` (optional): Process to view logs for ('ivry_server', 'ivry_cloudflared_tunnel', or 'all') (default: 'all')
- `--lines` (optional): Number of log lines to show (default: 20)
- `--project` (optional): Path to the project directory (default: current directory)

**Example:**

```bash
ivry_cli pm2_logs --process ivry_server --lines 50
```

**Response:**

```
=== ivry_server log (latest 50 lines) ===
2025-03-18 13:45:22 INFO Starting ivry_cli model server
2025-03-18 13:45:23 INFO Setting up server on port 3009
2025-03-18 13:45:23 INFO Server started successfully
...
```

## Utility Commands

### Find ComfyUI

Detect running ComfyUI instances, their installation paths, and ports.

```bash
ivry_cli find_comfyUI
```

**Example:**

```bash
ivry_cli find_comfyUI
```

**Response:**

```
detected ComfyUI process: PID=12345, name=python3
  comfyUI install path: /home/user/ComfyUI
  listening port: 8188
```

## Common Options and Flags

Most Ivry CLI commands support the following options:

- `--help`: Show help message for the command
- `--verbose`: Enable verbose output for debugging

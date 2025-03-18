# Troubleshooting

This guide addresses common issues you might encounter when using Ivry CLI and provides solutions to help you resolve them quickly.

## Installation Issues

### Module Not Found Error

**Problem:** After installation, running `ivry_cli` commands results in a "No module named 'ivry_cli'" error.

**Solutions:**

```bash
# Verify that the installation completed successfully
pip list | grep ivry_cli

# Try reinstalling with
pip install -e . --force-reinstall

# Check your Python path
python -c "import sys; print(sys.path)"
   
# If the ivry_cli directory is not in the path, add it manually
export PYTHONPATH=$PYTHONPATH:/path/to/ivry_cli
```

### Cloudflared Installation Issues

**Problem:** The `cloudflared` command is not found after installation.

**Solutions:**

```bash
# For Debian/Ubuntu
wget https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-amd64.deb
sudo dpkg -i cloudflared-linux-amd64.deb

# For other distributions
curl -L --output cloudflared https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-amd64
chmod +x cloudflared
sudo mv cloudflared /usr/local/bin
```

### Node.js and npm Issues

**Problem:** PM2 installation fails or commands like `pm2` are not found.

**Solutions:**

```bash
# Verify Node.js installation
node --version
npm --version

# If not installed, install Node.js and npm
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt install -y nodejs

# Install PM2 globally
sudo npm install -g pm2

# If permission issues occur
mkdir -p ~/.npm-global
npm config set prefix '~/.npm-global'
echo 'export PATH=~/.npm-global/bin:$PATH' >> ~/.bashrc
source ~/.bashrc
npm install -g pm2
```

## Authentication Issues

### API Key Problems

**Problem:** Authentication fails with "Invalid API key" errors.

**Solutions:**

```bash
# Check if your API key is properly saved
cat ~/.ivry/token.txt

# Re-authenticate with your API key
ivry_cli login --auth_token YOUR_API_KEY
```

### Expired or Invalid Token

**Problem:** Requests to the Ivry platform fail with authentication errors.

**Solutions:**

1. Login to the [Ivry website](https://www.ivry.co/login) to check your API key status
2. Generate a new API key if necessary
3. Re-authenticate with the new API key:

```bash
ivry_cli login --auth_token YOUR_NEW_API_KEY
```

## Server and Deployment Issues

### Port Already in Use

**Problem:** Starting the Ivry server fails with "Port 3009 is already in use" error.

**Solutions:**

```bash
# Find what's using port 3009
sudo lsof -i :3009

# Kill the process
sudo kill -9 <PID>

# Or force restart the server
ivry_cli run_server --force
```

### PM2 Process Management Issues

**Problem:** PM2 processes get stuck or don't start properly.

**Solutions:**

```bash
# Check PM2 process status
ivry_cli pm2_status

# Restart PM2 processes
ivry_cli pm2_control restart

# Or directly use PM2 commands
pm2 kill
pm2 resurrect

# Delete and recreate PM2 processes
ivry_cli stop_server --force
ivry_cli run_server --force
```

### Log File Size Issues

**Problem:** Log files grow too large and cause disk space problems.

**Solutions:**

```bash
# Check log sizes
ls -lh path/to/project/logs/

# Manually truncate logs if needed
truncate -s 0 path/to/project/logs/ivry_server.log
truncate -s 0 path/to/project/logs/cloudflared.log

# Restart services with log rotation enabled
ivry_cli stop_server
ivry_cli run_server
```

## ComfyUI Integration Issues

### ComfyUI Connection Failures

**Problem:** Ivry CLI cannot connect to ComfyUI or reports "ComfyUI server is not running".

**Solutions:**

```bash
# Check if ComfyUI is running
ivry_cli find_comfyUI

# Make sure ComfyUI is running with the --listen flag
# If using Windows, start ComfyUI with:
python main.py --listen

# Check port accessibility
curl http://127.0.0.1:8188 -I

# If using Windows+WSL, ensure the path is correctly converted
ivry_cli pull_project --app_id YOUR_APP_ID --comfyUI_dir "E:\\ComfyUI\\path" --comfyui_port 8188
```

### ComfyUI Workflow Issues

**Problem:** ComfyUI workflow errors or nodes not found.

**Solutions:**

1. Verify your workflow JSON is valid in ComfyUI directly
2. Check that all required custom nodes are installed
3. Ensure the workflow is properly formatted

```bash
# Validate the JSON format
cat comfyui_workflows/your_workflow.json | jq
```

4. Check for file path issues, especially when using Windows

## File Path Issues

### WSL-Windows Path Conversion Problems

**Problem:** Files cannot be found or accessed, especially when working between Windows and WSL.

**Solutions:**

For Windows paths that need conversion to WSL:

```bash
# Convert Windows path to WSL path (in WSL terminal)
wslpath 'C:\Users\YourName\Documents'

# Access Windows drives from WSL
ls /mnt/c/Users/YourName/Documents

# Make sure to use quotes for Windows paths with Ivry CLI
ivry_cli pull_project --app_id 123 --comfyUI_dir "E:\ComfyUI_windows_portable\ComfyUI"
```

For WSL paths that need conversion to Windows:

```powershell
# In PowerShell (requires WSL installed)
wsl pwd

# Access WSL files from Windows
explorer.exe \\wsl$\Ubuntu\home\username
```

## Networking Issues

### Cloudflare Tunnel Problems

**Problem:** Cloudflare tunnel fails to establish or reports errors.

**Solutions:**

```bash
# Check Cloudflare tunnel status
ivry_cli pm2_logs --process ivry_cloudflared_tunnel

# Verify tunnel configuration
cat tunnel_config.json

# Manually restart the tunnel
ivry_cli pm2_control restart --process ivry_cloudflared_tunnel

# If issues persist, try forcing a reconnection
cloudflared tunnel cleanup
ivry_cli stop_server --force
ivry_cli run_server --force
```

### Proxy or Firewall Issues

**Problem:** Network connections fail due to proxy or firewall restrictions.

**Solutions:**

1. Check your proxy settings
2. Ensure required ports (3009, 8188) are open
3. If behind a corporate firewall, request exceptions for the required services

```bash
# Test connectivity to Ivry servers
curl -v https://www.ivry.co/api/status

# Check if port 3009 is listening
ss -tulpn | grep 3009
```

## Environment and Python Issues

### Python Version Conflicts

**Problem:** Python version incompatibilities cause errors.

**Solutions:**

```bash
# Check your Python version
python --version

# Create a virtual environment with the correct Python version
python3.9 -m venv venv
source venv/bin/activate

# Install Ivry CLI in the virtual environment
pip install -e /path/to/ivry_cli
```

### Package Dependency Conflicts

**Problem:** Dependency conflicts between Ivry CLI and other installed packages.

**Solutions:**

```bash
# Create an isolated environment
python -m venv --clear ~/.venv/ivry_isolated
source ~/.venv/ivry_isolated/bin/activate

# Install Ivry CLI in the isolated environment
pip install -e /path/to/ivry_cli
```

## Debugging Techniques

### Enable Verbose Logging

To get more detailed information during troubleshooting:

```bash
# Set environment variable for debug logging
export COG_LOG_LEVEL=DEBUG
ivry_cli run_server

# Check detailed logs
ivry_cli pm2_logs --lines 100
```

### Test Components Individually

To identify which component is causing issues:

```bash
# Test ComfyUI connection
curl -v http://localhost:8188/

# Test Cloudflared connection
cloudflared tunnel info

# Test Ivry server directly
python -m cog.server.http --host=127.0.0.1 --port=3009
```

### Common Fixes Checklist

When troubleshooting, try these steps in order:

1. Restart the services: `ivry_cli stop_server && ivry_cli run_server`
2. Check log files for specific errors: `ivry_cli pm2_logs`
3. Verify network connectivity to required services
4. Ensure file paths are correctly formatted for your OS
5. Validate configuration files (tunnel_config.json, predict.py)
6. Check resource usage (CPU, memory, disk space)
7. Try with `--force` option: `ivry_cli run_server --force`

If problems persist after trying these solutions, please reach out to the Ivry support team with detailed information about your issue.
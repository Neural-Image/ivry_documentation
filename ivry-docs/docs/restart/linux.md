# **Linux restart Guide**

If your machine restarted or you want to restart the app, you can do following step:

## **Start App**

Specify a project path

 Please make sure your current path is at the root directory of the cli

 ```bash
 ivry_cli run_server --project project_folder_name --force #like app_30
 ```

or

Start both the ivry_cli model server and cloudflared tunnel:

 ```bash
 cd path/to/your/app
 ivry_cli run_server --force
 ```

If you are comfyUI user, also make sure your comfyUI is running with port using in cli (default is 8188)

## Troubleshot

**1. port 3009 is in use**

That might be caused by unproper stop of cli, you could kill the process in terminal and try to start the app again

check if port 3009 is in use:

```bash
lsof -i :3009
```

if you see something like:

```bash
COMMAND   PID USER   FD   TYPE             DEVICE SIZE/OFF NODE NAME
node     1234 youruser   22u  IPv4 0x...      0t0  TCP *:3009 (LISTEN)
```

then kill it by:

```bash
kill -9 PID # in this case is 'kill -9 1234'
```




# Jupyter Env

I use this env for hosting a jupyter server. I keep the env pretty spartan, only installing things in this env that improve the jupyterlab experience (e.g. things that facilitate rendering, extensions, etc).

## Usage

Whenever I reboot my homelab, I run the commands below to start serving jupyterlab in a `screen` session. The `--ip="$(tailscale ip -4)"` param tells the jupyter server to bind to the host machine's tailscale IP address (as I access this from another machine on my tailscale tailnet). 

```console
screen -S jupyter
conda activate jupyter_env
cd ~/projects
jupyter lab --no-browser --ip="$(tailscale ip -4)"
```

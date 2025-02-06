# Quarto Env

I use this env for locally rendering [my quarto blog posts](https://github.com/MattTriano/quarto_blog).

## Usage

I use the commands below to spin up a quarto server. If a port number isn't provided, quarto chooses a new one each time the server is restarted, so I just pick one so that I don't have to change the URL in my browser.

```console
screen -S quarto
cd /path/to/my/blog/dir
conda activate quarto_env
quarto preview --host $(tailscale ip -4) --port <some port number>
```

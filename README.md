Step-by-Step Guide: Local AI Stack (Ollama + OpenWebUI + Tailscale)
\
\
1. Install Ollama

Go to:

https://ollama.com

and install it.

For macOS and Linux:

curl -fsSL https://ollama.com/install.sh | sh
\
\
2. Pull a model

Choose a model based on your hardware:

Ollama Models

Then run:

ollama pull <model>

Example:

ollama pull llama3
\
\
3. Run your model

Start a model with:

ollama run <model>

Example:

ollama run llama3

This opens a local chat interface in your terminal.
\
\
4. Install Tailscale (remote access)

Download and install:

https://tailscale.com/download

Linux install:

curl -fsSL https://tailscale.com/install.sh | sh

Arch Linux:

sudo pacman -S tailscaled
sudo systemctl enable --now tailscaled

Then log in:

tailscale up

Install Tailscale on all devices you want to access this system from.
\
\
5. Install Docker

https://www.docker.com/get-started/

Arch Linux:

sudo pacman -S docker
sudo systemctl enable --now docker
\
\
6. Install OpenWebUI with Docker

Pull image:

docker pull ghcr.io/open-webui/open-webui:main

Run container:

docker run -d -p 3000:8080 -v open-webui:/app/backend/data --name open-webui ghcr.io/open-webui/open-webui:main
\
\
7. Test OpenWebUI

Open:

http://localhost:3000

Create your admin account on first launch.

This account is stored locally and is separate from Tailscale.
\
\
8. Expose via Tailscale

Run:

tailscale serve --bg 3000

You will get a URL like:

https://your-machine.tailnet.ts.net

You can access OpenWebUI from any device in your tailnet.
\
\
9. Connect OpenWebUI to Ollama

In OpenWebUI:

Settings → Admin Settings → Connections → Ollama API

Use:

http://127.0.0.1:11434
If Docker networking issues occur:
http://host.docker.internal:11434
\
\
Architecture

Internet
    │
Tailscale
    │
OpenWebUI (Docker, port 3000)
    │
Ollama (port 11434)
    │
Local Models

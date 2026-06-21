# Local AI Stack (Ollama + OpenWebUI + Tailscale)

This guide helps you set up a secure, private, and remotely accessible AI environment using **Ollama** for local LLMs, **OpenWebUI** as a web frontend, and **Tailscale** for secure remote access.

---

## Architecture Overview

```text
       [ Internet ]
            │
      ( Tailscale )
            │
   [ OpenWebUI (Docker) ]
         Port 3000
            │
     [ Ollama API ]
        Port 11434
            │
    [ Local Models ]
 (Llama 3, Gemma, etc.)
```

---

## 1. Install Ollama

Visit the official website:

https://ollama.com

Or install directly on macOS and Linux:

```bash
curl -fsSL https://ollama.com/install.sh | sh
```

---

## 2. Pull a Model

Choose a model that fits your available RAM and VRAM:

https://ollama.com/search

Download a model:

```bash
ollama pull <model_name>
```

Example:

```bash
ollama pull llama3
```

---

## 3. Run Your Model

Launch a model directly in the terminal:

```bash
ollama run <model_name>
```

Example:

```bash
ollama run llama3
```

To exit the chat interface:

```text
/exit
```

---

## 4. Install Tailscale

Tailscale allows secure remote access without opening ports on your router.

Download Tailscale:

https://tailscale.com/download

### Linux Installation

```bash
curl -fsSL https://tailscale.com/install.sh | sh
```

### Arch Linux Installation

```bash
sudo pacman -S tailscale
sudo systemctl enable --now tailscaled
```

Authenticate the machine:

```bash
sudo tailscale up
```

> Install Tailscale on every device you want to access your AI stack from.

---

## 5. Install Docker

Docker is used to run OpenWebUI.

Installation instructions:

https://www.docker.com/get-started/

### Arch Linux

```bash
sudo pacman -S docker
sudo systemctl enable --now docker
```

---

## 6. Install OpenWebUI

Pull the image:

```bash
docker pull ghcr.io/open-webui/open-webui:main
```

Run the container:

```bash
docker run -d \
  -p 3000:8080 \
  -v open-webui:/app/backend/data \
  --name open-webui \
  ghcr.io/open-webui/open-webui:main
```

This step can also be done by downloading the compose.yml file with:

```bash
docker compose up -d
```

---

## 7. Test OpenWebUI

Open:

```text
http://localhost:3000
```

Create an administrator account when prompted.

This account is stored locally and is independent of your Tailscale account.

---

## 8. Expose OpenWebUI via Tailscale

Expose the web interface securely through your tailnet:

```bash
tailscale serve --bg 3000
```

Tailscale will generate a URL similar to:

```text
https://your-machine-name.tailnet-id.ts.net
```

You can now access OpenWebUI from any authorized device connected to your tailnet.

---

## 9. Connect OpenWebUI to Ollama

If your models do not appear automatically:

1. Open **Settings**
2. Navigate to **Admin Settings → Connections → Ollama API**
3. Enter:

```text
http://127.0.0.1:11434
```

If Docker networking prevents OpenWebUI from reaching Ollama, try:

```text
http://host.docker.internal:11434
```

---

## Troubleshooting

### Check that Ollama is running

```bash
curl http://127.0.0.1:11434/api/tags
```

If successful, Ollama will return a JSON response containing your installed models.

### Check OpenWebUI container status

```bash
docker ps
```

### Check Tailscale status

```bash
tailscale status
```

---

## Result

You now have:

* Local LLMs running through Ollama
* A web interface provided by OpenWebUI
* Secure remote access through Tailscale
* No port forwarding required
* Full ownership of your data and models


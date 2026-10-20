# Speech server on your own computer

Whisper (accurate speech-to-text) is heavy to run next to VRChat. This guide puts
it on a **spare computer at home**, like an old laptop, so your gaming PC doesn't
have to do it. It's written for a laptop with **Ubuntu Server** (command line
only), such as an old Acer Aspire, but any Linux machine works.

```
Gaming PC (VRCompanion)  ──── home network ────▶  Laptop (Speaches = Whisper)
  wake word + commands stay here                   turns chat sentences into text
```

- Your gaming PC keeps doing the wake word and commands (instant, light).
- The laptop only gets the few seconds of audio of each chat sentence (and
  captions, and AI brain commands, if you use those).
- If the laptop is off, chat mode falls back to your PC. Nothing breaks.

It's free: [Speaches](https://github.com/speaches-ai/speaches) (the server) and
the Whisper models are open source.

## The quick way: one command

You need a spare computer on the same home network as your gaming PC, with
Linux (Ubuntu or Debian is easiest; Ubuntu Server is fine) or Windows 10/11.
On Windows, see [below](#on-a-windows-pc); the rest of this section is for Linux.

1. In VRCompanion: **Settings → Listening → Chat mode engine → Whisper on
   another computer / cloud**, then **Set up a server…**. It copies one long
   command.
2. On the spare computer, open a terminal (or log in from your PC with `ssh`,
   see [step 1](#step-1-log-in-to-the-laptop-from-your-pc) below), paste the
   command (right-click or Ctrl+Shift+V) and press Enter.

   It asks for your password (to install things), then does everything itself:
   installs Docker, keeps a laptop running with the lid closed, uses an NVIDIA
   card if there is one (and falls back to the CPU if it doesn't work), picks a
   Whisper model that fits the computer, makes a key, and starts the server so it
   comes back by itself after a restart. The first time takes 5–20 minutes,
   mostly downloading.

   With an NVIDIA card that has no driver yet, it installs the driver and asks
   you to restart (`sudo reboot`); paste the same command again afterwards.
3. At the end it prints a **setup code** (`VRCS-...`). Paste it into **Speech
   server** in VRCompanion: the address, model and key fill themselves in.
   Press **Test**, then **Save**.

**Updating:** paste the same command again (or get a fresh one from the app).
It keeps your key and model, so the app needs no changes.

**Choosing the model yourself:** put it in front of the last `bash`, e.g.
`... | gunzip | MODEL=Systran/faster-whisper-base bash` (see the table in
[step 3](#step-3-check-what-graphics-it-has)). `PORT=8001` changes the port,
`GPU=0` skips the NVIDIA card.

**Lost the code?** Paste the command again; it prints it again.

**Removing it:** paste the command, but change the end from `| bash` to
`| bash -s -- uninstall`.

### On a Windows PC

1. In VRCompanion: **Set up a server…** (as above), pick **Windows**. It copies
   the command.
2. On the spare PC, open **PowerShell** (Start menu, type PowerShell), paste the
   command (right-click) and press Enter. Allow it to make changes when Windows
   asks: a new window opens and does the rest.

   It downloads [uv](https://docs.astral.sh/uv/) (which installs its own
   Python, so you don't need one), installs faster-whisper (with the NVIDIA
   libraries if the PC has an NVIDIA card and driver; it falls back to the CPU
   if the card doesn't work), picks a model, makes a key, starts the server as
   a background task that starts with Windows (even before anyone logs in),
   opens the port in the Windows firewall, and stops the PC from going to sleep
   while it's plugged in.
3. It prints the **setup code**: paste it into **Speech server**, as above.

Everything lives in `C:\ProgramData\VRCompanionServer` (the log is
`server.log` there). Paste the command again to update. Options go at the very
end of the command, after `-File $f`: `-Model Systran/faster-whisper-base`,
`-Port 8001`, `-Cpu`, or `-Uninstall` to remove it.

The Windows server is VRCompanion's own small server
(server.py), which speaks the
same language as Speaches, so the app treats them the same. Windows needs
nothing else; the Linux way uses Speaches in Docker.

---

The long command is the script
install.sh (Windows:
install.ps1), packed into one line
(so it works without downloading anything from GitHub); read it first if you
like. The rest of this page does the same steps by hand.

---

## Doing it by hand (Linux)

**Time needed:** about 30 minutes, most of it waiting for downloads.

---

## Step 1: Log in to the laptop from your PC

Working from your PC is easier than typing on the laptop. On the laptop, log in
and find its address:

```bash
hostname -I
```

It prints something like `192.168.1.50`. On your PC, open **PowerShell** and
connect (use your laptop's user name):

```powershell
ssh sandie@192.168.1.50
```

Type `yes` the first time, then the laptop's password. Everything below is typed
into this window.

> If `ssh` says "connection refused", install the SSH server on the laptop first:
> `sudo apt install -y openssh-server`.

**Tip:** in your router, give the laptop a fixed address (often called *DHCP
reservation*), so it's always `192.168.1.50`.

## Step 2: Keep it running with the lid closed

```bash
sudo sed -i 's/^#\?HandleLidSwitch=.*/HandleLidSwitch=ignore/' /etc/systemd/logind.conf
sudo sed -i 's/^#\?HandleLidSwitchExternalPower=.*/HandleLidSwitchExternalPower=ignore/' /etc/systemd/logind.conf
sudo systemctl restart systemd-logind
```

Keep it plugged in. If its BIOS has an option to stop charging at 80%, turn it
on: that's kinder to a battery that's always on the charger.

## Step 3: Check what graphics it has

```bash
lspci | grep -i -E "vga|3d"
```

- Only **Intel** or **AMD**: you'll use the CPU version. Skip the GPU parts below.
- **NVIDIA** (e.g. GeForce 940MX, MX150): you can use the GPU version, which is
  several times faster.

Which model to use:

| The laptop has | Model | Time per sentence (rough) |
|---|---|---|
| No NVIDIA GPU | `Systran/faster-whisper-small` (or `-base` if the CPU is very old) | 1–3 s |
| NVIDIA GPU, 2 GB | `Systran/faster-whisper-small` or `-medium` | under 1 s |
| NVIDIA GPU, 4 GB or more | `deepdml/faster-whisper-large-v3-turbo-ct2` | under 1 s |

4 GB of RAM is enough for `small`; 8 GB for the bigger ones. Not sure? Start with
`small`. The app's **Test** button tells you the real speed.

## Step 4: Install Docker

Docker runs the server in a box that's easy to update and restarts by itself.

```bash
sudo apt update
sudo apt install -y docker.io
sudo usermod -aG docker $USER
```

Log out (`exit`) and `ssh` in again, so the last line takes effect. Check:

```bash
docker run --rm hello-world
```

It should print "Hello from Docker!".

### Only with an NVIDIA GPU

Install the driver and reboot:

```bash
sudo ubuntu-drivers install
sudo reboot
```

Wait a minute, `ssh` in again, and check that `nvidia-smi` shows the GPU. Then let
Docker use it (the NVIDIA Container Toolkit):

```bash
curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg
curl -s -L https://nvidia.github.io/libnvidia-container/stable/deb/nvidia-container-toolkit.list | \
  sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | \
  sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list
sudo apt update && sudo apt install -y nvidia-container-toolkit
sudo nvidia-ctk runtime configure --runtime=docker
sudo systemctl restart docker
```

(If these commands have changed, NVIDIA's
[install guide](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html)
has the current ones.)

## Step 5: Start the speech server

Make up a key, so only you can use the server:

```bash
openssl rand -hex 16
```

Copy what it prints. In the command below, replace `PUT-YOUR-KEY-HERE` with it,
and the model name with the one from step 3 if you picked another.

**Without an NVIDIA GPU:**

```bash
docker run --detach --restart unless-stopped --name speaches \
  --publish 8000:8000 \
  --volume hf-hub-cache:/home/ubuntu/.cache/huggingface/hub \
  --env API_KEY=PUT-YOUR-KEY-HERE \
  --env STT_MODEL_TTL=-1 \
  --env 'PRELOAD_MODELS=["Systran/faster-whisper-small"]' \
  ghcr.io/speaches-ai/speaches:latest-cpu
```

**With an NVIDIA GPU:** the same, with `--gpus=all` added and `latest-cuda` at
the end:

```bash
docker run --detach --restart unless-stopped --name speaches \
  --gpus=all \
  --publish 8000:8000 \
  --volume hf-hub-cache:/home/ubuntu/.cache/huggingface/hub \
  --env API_KEY=PUT-YOUR-KEY-HERE \
  --env STT_MODEL_TTL=-1 \
  --env 'PRELOAD_MODELS=["Systran/faster-whisper-small"]' \
  ghcr.io/speaches-ai/speaches:latest-cuda
```

What the options do: `--restart unless-stopped` starts it again after a reboot;
`STT_MODEL_TTL=-1` keeps the model loaded so the first sentence isn't slow;
`PRELOAD_MODELS` downloads the model on the first start (a few hundred MB).

Watch it start (Ctrl+C stops watching, not the server):

```bash
docker logs -f speaches
```

Wait until the model download is finished and it says it's running.

> **Hugging Face:** the models come from [Hugging Face](https://huggingface.co).
> You don't need an account, a token or a license for them: they're public, and
> Whisper is MIT licensed (free, also for commercial use). If the log mentions
> `HF_TOKEN` or not being logged in, that's only a notice about download speed
> limits; ignore it.

## Step 6: Check it from your PC

In PowerShell on your PC (not the ssh window), with your address and key:

```powershell
curl.exe http://192.168.1.50:8000/v1/models -H "Authorization: Bearer PUT-YOUR-KEY-HERE"
```

A list with your model in it means it works.

## Step 7: Connect VRCompanion

In the app: **Settings → Listening → Chat mode engine → Whisper on another
computer / cloud**, press **My own server**, then fill in:

- **Speech server:** `http://192.168.1.50:8000` (your laptop's address)
- **Model:** the model you started it with, e.g. `Systran/faster-whisper-small`
- **API key:** your key from step 5

Press **Test**: it says how many milliseconds a sentence took. Then **Save**.

Chat mode, [captions](in-vr.md#live-captions) and the [AI brain](ai-brain.md) now
use the laptop.

## Optional: use it away from home

[Tailscale](https://tailscale.com) is a free private network between your own
devices; nothing is opened to the internet. On the laptop:

```bash
curl -fsSL https://tailscale.com/install.sh | sh
sudo tailscale up
```

Open the link it prints and log in. Install Tailscale on your gaming PC too, with
the same account. Then use the laptop's Tailscale name as the server address, e.g.
`http://acer-laptop:8000`.

## Keeping it up to date

```bash
docker pull ghcr.io/speaches-ai/speaches:latest-cpu     # or latest-cuda
docker rm -f speaches
```

Then run the `docker run` command from step 5 again. The model stays downloaded.

## Troubleshooting

- **"Can't reach the server":** is the laptop on and is `speaches` in `docker ps`?
  Is the address right (`hostname -I`)? If you turned on Ubuntu's firewall, allow
  the port: `sudo ufw allow 8000/tcp`.
- **"The server refused the API key":** the key in the app must be exactly the
  `API_KEY` from step 5.
- **Slow:** try a smaller model, or check `docker logs speaches` to see whether it
  found the GPU.
- **It stopped after a reboot:** `docker ps -a` shows it; `docker start speaches`
  starts it. The `--restart unless-stopped` option should do that by itself.

## Letting other people use it

To give friends their own keys and limits, add the gateway on top:
Hosting a server for other people.

## No spare computer?

In the same setting, press **OpenAI** or **Groq** and paste an API key from that
service. It costs a fraction of a cent per minute (Groq has a free tier), but
your voice goes to that company.

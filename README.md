# Local file sharing servers

A short list of lightweight tools for sharing a folder on your LAN. Useful when you need to move files between devices, or spin up a temporary download and upload page, without standing up a full file server.

Pick one, `cd` into the folder you want to share, and run the command. Then open the printed URL from another device on the same network.

> These tools listen on your network interface. Treat them as temporary. Prefer a password, do not port-forward them, and stop the process when you are done.

## Which one should I use?

| Tool | Best for | Uploads | Install weight |
| --- | --- | --- | --- |
| [Updog](#updog) | A nicer Python HTTP server, fast to start | Yes | Tiny (`uv` or `pip`) |
| [Miniserve](#miniserve) | Fast transfers, QR code, search | Optional | One binary |
| [File Browser](#file-browser) | A small private Drive-style UI | Yes, with users | Docker |
| [Droopy](#droopy) | "Just let people upload files here" | Upload only | Tiny (`uv` or `pip`) |

Default examples below use port **8080**. Change it if that port is already taken.

## Updog

A small Python server with a clean directory page and uploads. Good default when you already have Python tooling.

### Install

No install needed if you have [uv](https://docs.astral.sh/uv/):

```bash
uvx updog --help
```

Or install it for repeated use:

```bash
pipx install updog
# or: uv tool install updog
```

### Share this folder

```bash
uvx updog --port 8080
```

With a password:

```bash
uvx updog --port 8080 --password 'change-me'
```

Useful flags: `--directory`, `--password`, `--ssl`. Run `uvx updog --help` for the current list.

## Miniserve

A fast Rust file server. Nice directory listings, optional uploads, and a QR code so a phone can join without typing the IP.

### Install

Prefer a current binary over the distro package. Distro builds are often old.

```bash
# macOS
brew install miniserve

# Rust toolchain
cargo install miniserve

# or grab a release binary:
# https://github.com/svenstaro/miniserve/releases
```

### Share this folder

Read-only:

```bash
miniserve --port 8080 .
```

Allow uploads:

```bash
miniserve --port 8080 --upload-files .
```

With HTTP basic auth (`-a` / `--auth` can be repeated):

```bash
miniserve --port 8080 --upload-files --auth 'alice:change-me' .
```

On Debian or Ubuntu, `sudo apt install miniserve` works, but check `miniserve --version` before relying on newer flags.

## File Browser

A web file manager: users, drag-and-drop, previews, and a text editor. Heavier than the others, and the right pick if more than one person will use the share.

### Requirements

Docker (or a current File Browser binary from the project releases).

### Share this folder

This keeps the database and config in Docker volumes so logins survive a restart:

```bash
docker run -d \
  --name filebrowser \
  --restart unless-stopped \
  -p 8080:80 \
  -v "$PWD":/srv \
  -v filebrowser_database:/database \
  -v filebrowser_config:/config \
  filebrowser/filebrowser
```

Then open `http://<your-lan-ip>:8080`.

Stop and remove it when you are finished:

```bash
docker rm -f filebrowser
```

Set a real admin password in the UI before anyone else on the LAN uses it. Do not publish port 8080 on a public interface.

Official docs: https://filebrowser.org/

## Droopy

A tiny upload page. It is for receiving files, not for browsing a directory.

### Install

```bash
uvx --from droopy droopy --help
```

Or:

```bash
pipx install droopy
```

### Receive uploads in this folder

Port is a positional argument. Do not pass `-p` for the port. In Droopy, `-p` sets a picture.

```bash
uvx --from droopy droopy -d . -m "Drop files here" 8080
```

`-d` is the upload directory. `-m` is the message shown on the page. HTTPS is `--ssl` plus a PEM file. See `droopy --help`.

Droopy is older and less maintained than the other three. Use Updog or Miniserve if you also need downloads.

## Reach it from another device

1. Start the server.
2. On the host, find the LAN address:

```bash
# Linux
ip -4 addr show scope global

# macOS
ipconfig getifaddr en0
```

3. From the other device, open `http://<that-address>:8080`.
4. If it does not connect, allow the port in the host firewall, and confirm both devices are on the same network (guest Wi-Fi often blocks this).

Miniserve prints a QR code for this step.

## Quick comparison

| Tool | Start command | Uploads |
| --- | --- | --- |
| Updog | `uvx updog --port 8080` | Yes |
| Miniserve | `miniserve --port 8080 --upload-files .` | Opt-in |
| File Browser | Docker command above | Yes |
| Droopy | `uvx --from droopy droopy -d . 8080` | Upload only |

## Safety notes

- Bind these to a trusted LAN. A password is not a substitute for not exposing the port to the internet.
- Upload-enabled servers let peers write into the shared folder. Point them at a disposable directory.
- Stop the server when the transfer is done. For File Browser, `docker rm -f filebrowser` also drops the published port.
- For anything that must stay up, put it behind auth, TLS, and a reverse proxy. These one-liners are for short-lived shares.

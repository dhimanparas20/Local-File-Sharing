## Local File Sharing Servers
A curated collection of quick, lightweight, and awesome local file-sharing servers to spin up inside any directory. Ideal for transferring files between devices on the same network or hosting temporary web interfaces.
------------------------------
## 🚀 1. Updog
A beautifully simple, Python-based replacement for Python's built-in HTTP server that features an elegant UI and effortless file uploading.
## Installation
No global installation required if you use uv. If you prefer a traditional setup via pip:

pip install updog

## Usage
Run the following command in the directory you want to share:

uvx updog -p 8080


* Port: 8080
* Features: Supports file uploads through the web interface, basic authentication, and cleaner logging.

------------------------------
## 📂 2. Filebrowser
The ultimate option if you want a fully functional web file manager. It provides a desktop-like experience resembling a private Google Drive or Dropbox instance.
## Installation
Ensure you have Docker installed on your system. No further installation or binary downloads are required.
## Usage
Run the container and mount your current working directory ($PWD):

docker run -d -p 8080:80 -v "$PWD":/srv filebrowser/filebrowser


* Port: 8080 (maps container port 80 to host port 8080)
* Features: Responsive dark mode, grid/list file views, bulk drag-and-drop uploads, image/video gallery viewers, user authentication management, and an integrated text editor.

------------------------------
## ⚡ 3. Miniserve
A blazingly fast, modern CLI static file server written in Rust. It is highly optimized for performance and responsive directory listings.
## Installation
Since it is a compiled binary and not hosted on PyPI, install it via your system package manager:

# Debian / Ubuntu
sudo apt install miniserve

## Usage
To serve your current directory (.) and enable upload privileges, use:

miniserve -u -p 8080 .


* Port: 8080
* Flags: -u explicitly enables the upload form.
* Features: Automatic QR code generation for quick mobile access, code syntax highlighting, themes, and global search filtering.

------------------------------
## 💧 4. Droopy
An upload-first, extremely lightweight Python utility designed purely for letting other machines upload assets to your computer via a simple, un-cluttered interface.
## Installation
Can be run instantly via uv or installed using pip:

pip install droopy

## Usage
Execute the tool directly from PyPI via uvx:

uvx --from droopy droopy -p 8080


* Port: 8080
* Features: Minimalist interface focused entirely on handling incoming file streams, custom HTML page configurations, and password protection options.

------------------------------
## 📝 Quick Reference Summary

| Tool | Engine | Uploads | Best For |
|---|---|---|---|
| Updog | Python / PyPI | Yes | Rapid setups requiring a clean look |
| Filebrowser | Go / Docker | Yes (Advanced) | Full cloud storage simulation and management |
| Miniserve | Rust / APT | Yes (via -u) | High-speed transfers, QR generation, & search |
| Droopy | Python / PyPI | Yes | Purely receiving files with zero overhead |

Would you like me to add code snippets for configuring basic password protection or enabling HTTPS/SSL layers to this Markdown file?


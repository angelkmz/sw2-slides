## Requirements

You only need a modern web browser and a small local web server.

A local web server is recommended because browser security rules can block some presentation features when opening `index.html` directly from the filesystem.

You can use any one of these options:

- Python 3
- Node.js / npm
- PHP
- VS Code with the Live Server extension
- Any other static file server

## Quick start

Open a terminal in the presentation folder, the folder that contains `index.html` and `slides.md`.

Then start a local server using one of the methods below.

## Option 1: Python 3

This works on macOS, Linux, and Windows if Python 3 is installed.

```bash
python3 -m http.server 8000
```

On Windows, depending on your Python installation, use:

```powershell
py -m http.server 8000
```

or:

```powershell
python -m http.server 8000
```

Then open this URL in your browser:

```text
http://localhost:8000
```

## Option 2: Node.js

This works on macOS, Linux, and Windows if Node.js is installed.

```bash
npx serve .
```

The command will print a local URL, usually something like:

```text
http://localhost:3000
```

Open that URL in your browser.

If you prefer a fixed port:

```bash
npx serve . -l 8000
```

Then open:

```text
http://localhost:8000
```

## Option 3: PHP

This works on macOS, Linux, and Windows if PHP is installed.

```bash
php -S localhost:8000
```

Then open:

```text
http://localhost:8000
```

## Option 4: VS Code Live Server

1. Open the presentation folder in Visual Studio Code.
2. Install the **Live Server** extension if it is not already installed.
3. Right-click `index.html`.
4. Select **Open with Live Server**.
5. Your browser should open automatically.

## Linux distro notes

### Debian / Ubuntu

Install Python 3 if needed:

```bash
sudo apt update
sudo apt install python3
```

Then run:

```bash
python3 -m http.server 8000
```

### Fedora

Install Python 3 if needed:

```bash
sudo dnf install python3
```

Then run:

```bash
python3 -m http.server 8000
```

### Arch Linux

Install Python 3 if needed:

```bash
sudo pacman -S python
```

Then run:

```bash
python -m http.server 8000
```

## Windows notes

Open PowerShell or Windows Terminal in the presentation folder.

Try:

```powershell
py -m http.server 8000
```

If that does not work, try:

```powershell
python -m http.server 8000
```

Then open:

```text
http://localhost:8000
```

## macOS notes

Open Terminal in the presentation folder.

Python 3 may already be installed. Try:

```bash
python3 -m http.server 8000
```

Then open:

```text
http://localhost:8000
```

If Python 3 is not available, install it using Homebrew:

```bash
brew install python
```

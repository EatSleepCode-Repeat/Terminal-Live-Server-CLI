# 🚀 serve.zsh

<div align="center">

**A lightweight, zero-config local development server with live reload for zsh**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Shell: Zsh](https://img.shields.io/badge/Shell-Zsh-brightgreen.svg)](https://www.zsh.org/)
[![Python: 2.7+/3.x](https://img.shields.io/badge/Python-2.7%2B%20%7C%203.x-blue.svg)](https://www.python.org/)

*The simplicity of Python's HTTP server meets the power of live reload—all in your terminal.*

[Features](#-features) • [Installation](#-installation) • [Usage](#-usage) • [Examples](#-examples) • [Contributing](#-contributing)

</div>

---

## 📖 Overview

**serve.zsh** is a native zsh function that turns your terminal into a powerful development server. Inspired by VSCode's Live Server extension, it provides instant file serving with automatic browser refresh—no Node.js, no npm packages, just Python and zsh.

Perfect for:

- 🎨 Quick HTML/CSS/JS prototyping
- 📱 Testing responsive designs across devices
- 🔧 Local development without heavy tooling
- 🎓 Teaching and learning web development
- ⚡ Rapid iterations with live reload

---

## ✨ Features

| Feature | Description |
|---------|-------------|
| **🔄 Live Reload** | Automatically refreshes your browser when HTML files change |
| **⚡ Zero Config** | Works instantly—no configuration files needed |
| **🌐 Network Access** | Access your site from any device on your network |
| **🎯 Port Management** | Smart port conflict detection and custom port selection |
| **🚀 Auto-Open** | Optional browser launch on server start |
| **🧹 Process Control** | Easy cleanup of running server instances |
| **📦 Minimal Dependencies** | Only requires Python (2.7+ or 3.x) |
| **🎨 Beautiful CLI** | Clean, colorful terminal output |

---

## 📦 Installation

### Step 1: Create the script directory

```zsh
mkdir -p ~/zsh-scripts
```

### Step 2: Download the script

**Option A: Clone the repository**

```zsh
git clone https://github.com/eatsleepcode-repeat/serve.zsh.git
cp serve.zsh/serve.zsh ~/zsh-scripts/
```

**Option B: Direct download**

```zsh
curl -o ~/zsh-scripts/serve.zsh https://raw.githubusercontent.com/eatsleepcode-repeat/serve.zsh/main/serve.zsh
```

**Option C: Manual installation**

```zsh
nano ~/zsh-scripts/serve.zsh
# Paste the script content and save
```

### Step 3: Make it executable (optional)

```zsh
chmod +x ~/zsh-scripts/serve.zsh
```

### Step 4: Source it in your `.zshrc`

```zsh
echo "source ~/zsh-scripts/serve.zsh" >> ~/.zshrc
source ~/.zshrc
```

### Verify Installation

```zsh
serve --help  # Should show usage information
```

---

## 🎯 Usage

### Basic Commands

```zsh
# Serve current directory on port 8000
serve

# Serve on custom port
serve 3000

# Serve specific directory
serve 8080 ~/my-project

# Serve and auto-open browser
serve-open

# Serve specific directory and open
serve-open 3000 ~/my-website

# Kill all running serve processes
serve-kill
```

### Command Reference

| Command | Arguments | Description |
|---------|-----------|-------------|
| `serve` | `[port] [directory]` | Start server with live reload |
| `serve-open` | `[port] [directory]` | Start server and open browser |
| `serve-kill` | none | Kill all active server processes |

**Default Values:**

- Port: `8000`
- Directory: `.` (current directory)

---

## 💡 Examples

### Example 1: Quick HTML Preview

```zsh
# Create a simple HTML file
echo "<h1>Hello World!</h1>" > index.html

# Start server
serve

# Open http://localhost:8000 in your browser
# Edit index.html - browser auto-refreshes! 🎉
```

### Example 2: Multi-Device Testing

```zsh
# Start server
serve 8000 ~/my-responsive-site

# Output shows:
# 🌐 Local: http://localhost:8000
# 📡 Network: http://192.168.1.100:8000

# Now access from phone/tablet using the network URL!
```

### Example 3: Multiple Projects

```zsh
# Terminal 1: Project A
cd ~/project-a
serve 3000

# Terminal 2: Project B
cd ~/project-b
serve 4000

# Both running simultaneously!
```

### Example 4: Clean Workspace

```zsh
# List and kill all server processes
serve-kill

# Output:
# 🔍 Looking for active serve processes...
# Found processes on ports 8000-9000
# Kill these processes? (y/n):
```

---

## 🔧 How It Works

### Live Reload Mechanism

serve.zsh injects a lightweight JavaScript snippet into HTML files that:

1. **Polls for changes** every 500ms using HTTP HEAD requests
2. **Compares timestamps** via the `Last-Modified` header
3. **Triggers reload** when changes are detected
4. **Logs activity** to browser console for debugging

```javascript
// The injected live reload script
🔄 Live reload enabled
🔄 Changes detected, reloading...
```

### Server Architecture

```
┌─────────────────┐
│   Browser       │
│  (Live Reload)  │
└────────┬────────┘
         │ HTTP
         ▼
┌─────────────────┐
│  Python HTTP    │
│    Server       │
│ (Custom Handler)│
└────────┬────────┘
         │ File Access
         ▼
┌─────────────────┐
│  Local Files    │
│   (HTML/CSS/JS) │
└─────────────────┘
```

---

## 🎨 Configuration

### Adjusting Reload Speed

Edit the polling interval in `serve.zsh`:

```javascript
// Default: 500ms (line ~95)
setInterval(checkForChanges, 500);

// Fast: 250ms (more responsive, more requests)
setInterval(checkForChanges, 250);

// Slow: 1000ms (less responsive, fewer requests)
setInterval(checkForChanges, 1000);
```

### Custom Port Range for `serve-kill`

```zsh
# Default: ports 8000-9000 (line ~151)
local pids=$(lsof -ti:8000-9000 2>/dev/null | sort -u)

# Custom range: ports 3000-5000
local pids=$(lsof -ti:3000-5000 2>/dev/null | sort -u)
```

---

## 🐛 Troubleshooting

### Issue: "Port already in use"

```zsh
# Find what's using the port
lsof -i :8000

# Kill the process
kill -9 <PID>

# Or use serve-kill
serve-kill
```

### Issue: "Python is not installed"

```zsh
# Check Python installation
python3 --version
# or
python --version

# Install Python (macOS with Homebrew)
brew install python3

# Install Python (Linux)
sudo apt install python3  # Debian/Ubuntu
sudo yum install python3  # RedHat/CentOS
```

### Issue: Live reload not working

- Ensure you're editing HTML files (not just CSS/JS)
- Check browser console for live reload messages
- Verify the HTML file has a `</body>` tag for script injection
- Try hard refresh: `Cmd+Shift+R` (Mac) or `Ctrl+Shift+R` (Windows/Linux)

### Issue: Network address not showing

```zsh
# Manually check your IP
ipconfig getifaddr en0  # macOS
hostname -I             # Linux
```

---

## 🤝 Contributing

Contributions are welcome! Here's how you can help:

### Reporting Bugs

Open an issue with:

- Your OS and zsh version (`zsh --version`)
- Python version (`python3 --version`)
- Steps to reproduce
- Expected vs actual behavior

### Suggesting Features

Have ideas? Open an issue with:

- Feature description
- Use case / why it's useful
- Proposed implementation (optional)

### Pull Requests

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

---

## 📜 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

```
MIT License

Copyright (c) 2025 [Chris Clines]

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.
```

---

## 🙏 Acknowledgments

- Inspired by [VSCode Live Server](https://github.com/ritwickdey/vscode-live-server) extension
- Built on Python's robust `http.server` module
- Made with ❤️ for the developer community

---

## 📊 Project Stats

<div align="center">

**⭐ If you find this useful, please consider starring the repository!**

Made with 🔥 by developers, for developers

</div>

---

<div align="center">

**Happy Coding! 🚀**

[⬆ Back to Top](#-servezsh)

</div>

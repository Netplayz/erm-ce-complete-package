# 🚀 WebGUI Quick Start Guide

## What is the WebGUI?

A real-time monitoring dashboard that shows live CPU usage, memory, disk space, and bot statistics in your web browser!

## Quick Setup (3 Steps)

### Step 1: Install Dependencies

```bash
pip install psutil uvicorn fastapi
```

Or just use:
```bash
pip install -r requirements.txt
```

### Step 2: Configure (Optional)

Add to your `.env` file:

```env
WEBGUI_HOST=0.0.0.0
WEBGUI_PORT=8080
```

*You can skip this step - it uses these defaults automatically!*

### Step 3: Start the Bot

```bash
python3 erm.py
```

Look for this message in the logs:
```
Starting WebGUI Dashboard on http://0.0.0.0:8080
```

## Access the Dashboard

**On the same computer:**
```
http://localhost:8080
```

**From another computer on your network:**
```
http://YOUR_SERVER_IP:8080
```

## What You'll See

### 📊 Real-Time Stats
- CPU usage with live graphs
- Memory and disk usage
- Network statistics
- Bot process information

### 🤖 Bot Info
- Number of guilds and users
- Bot uptime
- API latency
- Connection status

### 📈 Live Graphs
- CPU usage history (last 60 seconds)
- Memory usage history (last 60 seconds)
- Auto-updates every second!

## Screenshots

### Main Dashboard
```
╔═══════════════════════════════════════════════════════╗
║  🟢 ERM-CE Bot Dashboard     🟢 Connected             ║
╠═══════════════════════════════════════════════════════╣
║  🤖 Bot Information                                   ║
║  ┌─────────┬─────────┬─────────┬─────────┐          ║
║  │ Guilds  │  Users  │ Uptime  │ Latency │          ║
║  │   42    │  12.5K  │ 5h 23m  │  45ms   │          ║
║  └─────────┴─────────┴─────────┴─────────┘          ║
╠═══════════════════════════════════════════════════════╣
║  💻 CPU Usage: 23.4%        🧠 Memory: 45.2%          ║
║  ████████░░░░░░░░░░          ████████████░░░░░░░      ║
╠═══════════════════════════════════════════════════════╣
║  📊 CPU History                📈 Memory History      ║
║  [Live updating line graphs showing last 60 seconds] ║
╚═══════════════════════════════════════════════════════╝
```

## Troubleshooting

### ❌ Dashboard won't start

**Error: "Failed to start WebGUI Dashboard"**

```bash
# Install missing packages
pip install psutil uvicorn fastapi
```

### ❌ Can't connect to dashboard

**"Connection refused" in browser**

1. Check bot is running: `ps aux | grep erm.py`
2. Check port is open: `lsof -i :8080`
3. Try different port: Set `WEBGUI_PORT=8081` in `.env`

### ❌ Shows "Disconnected"

**WebSocket connection issues**

1. Refresh the page (F5)
2. Check firewall isn't blocking WebSocket
3. Try different browser

## Tips & Tricks

### 💡 Customize the Port
```env
WEBGUI_PORT=9000  # Use any available port
```

### 💡 Local Access Only
```env
WEBGUI_HOST=127.0.0.1  # Only accessible from same machine
```

### 💡 Access from Phone/Tablet
1. Connect device to same WiFi
2. Find server IP: `hostname -I`
3. Open browser: `http://SERVER_IP:8080`

### 💡 Multiple Browser Tabs
Open multiple tabs - each gets real-time updates!

### 💡 Use REST API
```bash
# Get stats as JSON
curl http://localhost:8080/api/system
curl http://localhost:8080/api/bot
curl http://localhost:8080/api/combined
```

## What's Monitored?

### System Stats
- ✅ CPU percentage and cores
- ✅ Memory (RAM + Swap)
- ✅ Disk space
- ✅ Network traffic
- ✅ System uptime

### Bot Stats
- ✅ Guild count
- ✅ User count
- ✅ Bot uptime
- ✅ API latency
- ✅ Connection status

### Process Stats
- ✅ Bot memory usage
- ✅ Bot CPU usage
- ✅ Thread count

## Features

- 🔄 **Auto-refresh**: Updates every second
- 📱 **Responsive**: Works on desktop, tablet, mobile
- 🎨 **Modern UI**: Beautiful gradient design
- 📊 **Live Graphs**: Real-time Chart.js visualizations
- 🔌 **WebSocket**: Efficient real-time communication
- 🎯 **Lightweight**: Minimal resource usage

## Advanced Usage

### Port Forwarding (Access from Internet)

1. Forward port 8080 in your router settings
2. Find public IP: https://whatismyipaddress.com
3. Access: `http://YOUR_PUBLIC_IP:8080`

⚠️ **Warning**: Only do this if you understand security implications!

### Reverse Proxy (Add HTTPS)

**Nginx example:**
```nginx
server {
    listen 443 ssl;
    server_name dashboard.yourdomain.com;
    
    location / {
        proxy_pass http://localhost:8080;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
    }
}
```

## Need Help?

📖 **Full Documentation**: See `webgui/README.md`  
🐛 **Bug Reports**: Open an issue on GitHub  
💬 **Questions**: Check the project Discord/forum

---

**Made with ❤️ for ERM-CE Bot**  
*Monitor your bot in style!* ✨

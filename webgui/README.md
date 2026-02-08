# 🖥️ ERM-CE WebGUI Dashboard

## Overview

The WebGUI Dashboard provides real-time system monitoring and bot statistics through a modern web interface. It displays live CPU usage, memory consumption, disk space, network activity, and bot performance metrics.

## Features

### 📊 Real-Time Monitoring
- **CPU Usage**: Current usage percentage, core count, and frequency
- **Memory Usage**: RAM and swap memory statistics
- **Disk Usage**: Storage capacity and free space
- **Network Stats**: Bytes sent/received and packet counts
- **Process Info**: Bot process memory and CPU usage

### 🤖 Bot Statistics
- Guild count and user count
- Bot uptime and latency
- Connection status
- Command count

### 📈 Live Graphs
- CPU usage history (last 60 seconds)
- Memory usage history (last 60 seconds)
- Real-time updates via WebSocket

### 🎨 Modern Interface
- Responsive design (works on desktop, tablet, and mobile)
- Dark gradient theme with smooth animations
- Color-coded progress bars (green/orange/red based on usage)
- Automatic reconnection on connection loss

## Installation

### 1. Install Required Dependencies

The WebGUI requires additional Python packages:

```bash
pip install psutil uvicorn fastapi
```

Or install from requirements.txt:

```bash
pip install -r requirements.txt
```

### 2. Configure Environment Variables

Add these optional settings to your `.env` file:

```env
# WebGUI Dashboard Settings
WEBGUI_HOST=0.0.0.0    # Listen on all interfaces (default)
WEBGUI_PORT=8080       # Port to run the dashboard on (default: 8080)
```

**Configuration Options:**

- `WEBGUI_HOST`: 
  - `0.0.0.0` - Listen on all network interfaces (accessible from other computers)
  - `127.0.0.1` - Listen only on localhost (only accessible from the same machine)
  - Default: `0.0.0.0`

- `WEBGUI_PORT`: 
  - Any available port (1024-65535)
  - Default: `8080`
  - Make sure the port is not used by another application

### 3. Start the Bot

The WebGUI starts automatically when the bot starts:

```bash
python3 erm.py
```

You'll see a log message:

```
Starting WebGUI Dashboard on http://0.0.0.0:8080
```

## Accessing the Dashboard

### From the Same Computer

Open your web browser and go to:

```
http://localhost:8080
```

### From Another Computer on the Same Network

1. Find your bot server's IP address:
   ```bash
   # Linux/Mac
   hostname -I
   
   # Windows
   ipconfig
   ```

2. Open browser on another computer and go to:
   ```
   http://YOUR_SERVER_IP:8080
   ```
   
   Example: `http://192.168.1.100:8080`

### From the Internet (Advanced)

To access the dashboard from outside your local network:

1. **Port Forwarding**: Configure your router to forward port 8080 to your server
2. **Find Public IP**: Visit https://whatismyipaddress.com
3. **Access Dashboard**: `http://YOUR_PUBLIC_IP:8080`

⚠️ **Security Warning**: Only expose the dashboard to the internet if you understand the security implications. Consider adding authentication or using a VPN.

## Dashboard Interface

### Header Section
- Bot name and online status (green dot = online, red dot = offline)
- Connection status indicator
- Last update timestamp

### Bot Information Card
- **Guilds**: Number of Discord servers the bot is in
- **Users**: Total number of users across all guilds
- **Uptime**: How long the bot has been running
- **Latency**: Discord API response time in milliseconds

### System Monitoring Cards

1. **CPU Usage**
   - Current CPU percentage
   - Number of logical cores
   - Current and maximum CPU frequency
   - Real-time progress bar

2. **Memory Usage**
   - Used vs. total RAM
   - Available memory
   - Swap memory usage
   - Real-time progress bar

3. **Disk Usage**
   - Used vs. total disk space
   - Free space available
   - Real-time progress bar

4. **Bot Process**
   - Memory used by the bot process
   - CPU usage by the bot process
   - Number of active threads

5. **Network Statistics**
   - Total data sent
   - Total data received

6. **System Information**
   - Operating system and version
   - Hostname
   - System uptime

### Live Graphs
- **CPU Usage History**: Line graph showing CPU usage over the last 60 seconds
- **Memory Usage History**: Line graph showing memory usage over the last 60 seconds

## Technical Details

### Architecture

- **Backend**: FastAPI with WebSocket support
- **Frontend**: Vanilla HTML/CSS/JavaScript with Chart.js
- **Real-Time Updates**: WebSocket connection updates every second
- **System Monitoring**: psutil library for cross-platform system information

### WebSocket Protocol

The dashboard uses WebSocket for real-time updates:

```javascript
// Connection: ws://localhost:8080/ws
// Message types:
{
  "type": "initial",  // First message on connection
  "system": { ... },  // System statistics
  "bot": { ... }      // Bot statistics
}

{
  "type": "update",   // Periodic updates (every 1 second)
  "system": { ... },
  "bot": { ... }
}
```

### REST API Endpoints

The dashboard also provides REST API endpoints:

- `GET /` - Dashboard HTML interface
- `GET /api/system` - Current system statistics (JSON)
- `GET /api/bot` - Current bot statistics (JSON)
- `GET /api/combined` - Both system and bot statistics (JSON)
- `WebSocket /ws` - Real-time updates

### Example API Usage

```bash
# Get system stats
curl http://localhost:8080/api/system

# Get bot stats
curl http://localhost:8080/api/bot

# Get combined stats
curl http://localhost:8080/api/combined
```

## Troubleshooting

### Dashboard Won't Start

**Issue**: "Failed to start WebGUI Dashboard"

**Solutions**:
1. Install missing dependencies:
   ```bash
   pip install psutil uvicorn fastapi
   ```

2. Check if port is already in use:
   ```bash
   # Linux/Mac
   lsof -i :8080
   
   # Windows
   netstat -ano | findstr :8080
   ```

3. Try a different port in `.env`:
   ```env
   WEBGUI_PORT=8081
   ```

### Can't Connect to Dashboard

**Issue**: Browser shows "Connection refused" or "Cannot connect"

**Solutions**:
1. Verify the bot is running and WebGUI started successfully
2. Check firewall settings:
   ```bash
   # Linux: Allow port through firewall
   sudo ufw allow 8080/tcp
   
   # Windows: Add inbound rule in Windows Firewall
   ```
3. Check if the correct IP address is being used
4. Try accessing via `http://127.0.0.1:8080` on the same machine first

### WebSocket Connection Issues

**Issue**: Dashboard shows "Disconnected" or keeps reconnecting

**Solutions**:
1. Check browser console for errors (F12)
2. Verify WebSocket support in your browser
3. Check if a proxy or firewall is blocking WebSocket connections
4. Try a different browser

### Dashboard Shows Old Data

**Issue**: Statistics not updating or updating slowly

**Solutions**:
1. Refresh the page (F5 or Ctrl+R)
2. Check WebSocket connection status in the header
3. Check bot logs for errors
4. Clear browser cache and reload

## Performance Considerations

### Resource Usage

The WebGUI is lightweight and has minimal impact:
- **Memory**: ~20-30 MB additional RAM usage
- **CPU**: <1% additional CPU usage
- **Network**: ~1 KB/s per connected client

### Scaling

- The WebGUI can handle multiple simultaneous connections
- Each connected browser tab receives real-time updates
- Updates are sent via WebSocket broadcast to all clients

### Recommendations

- **Production**: Access via localhost or secure network
- **Development**: Can be accessed from any network interface
- **Multiple Instances**: Each bot instance needs a unique port

## Security Best Practices

1. **Don't Expose Publicly**: Keep the dashboard on your local network
2. **Use VPN**: Access remotely via VPN instead of port forwarding
3. **Firewall**: Restrict access to specific IP addresses
4. **HTTPS**: Consider adding a reverse proxy with SSL (nginx, Caddy)
5. **Authentication**: Add basic auth via reverse proxy if needed

## Future Enhancements

Potential features for future versions:
- [ ] User authentication
- [ ] Historical data storage and graphs
- [ ] Bot command execution from dashboard
- [ ] Alert notifications for high resource usage
- [ ] Guild-specific statistics
- [ ] Log viewer
- [ ] Configuration editor

## Support

If you encounter issues with the WebGUI Dashboard:

1. Check this documentation
2. Review bot logs for error messages
3. Ensure all dependencies are installed
4. Check your firewall and network settings
5. Open an issue on the project repository

## Credits

- **FastAPI**: Modern web framework for the backend
- **Chart.js**: Beautiful charts for data visualization
- **psutil**: Cross-platform system and process monitoring

---

**Version**: 1.0.0  
**Last Updated**: February 2026  
**Compatible With**: ERM-CE Bot v4+

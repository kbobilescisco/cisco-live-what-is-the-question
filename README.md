# CCNP AUTOCOR Jeopardy

Interactive Jeopardy game for Cisco Live session [IBOCRT-2775] - CCNP Automation certification prep with real-time team buzzers.

## Features
- 🎮 Interactive Jeopardy-style game board with custom team icons
- 📱 Mobile buzzer system - teams buzz in from their phones
- 🔴 Real-time MQTT communication over WebSockets (works on enterprise WiFi)
- 🏆 Live score tracking for 5 teams (Ansible, Terraform, Python, YANG, pyATS)
- 🎯 Multiple game sessions with different question sets
- 🚀 No installation required - runs in browser

## Quick Start

### 1. Start Local Server
```bash
python3 -m http.server 8000
```

### 2. Test Buzzer System (Before Session)
Open the test page to verify everything works:
```
http://localhost:8000/test-buzzers.html
```
- Test all 5 team buzzers
- Monitor MQTT messages in real-time
- Copy team URLs for QR code generation
- Verify host display receives buzzes

### 3. Create QR Codes for Teams
Use the URLs from the test page (or manually construct):
- **Team Ansible:** `http://YOUR-IP:8000/buzzer.html?team=ansible`
- **Team Terraform:** `http://YOUR-IP:8000/buzzer.html?team=terraform`
- **Team Python:** `http://YOUR-IP:8000/buzzer.html?team=python`
- **Team YANG:** `http://YOUR-IP:8000/buzzer.html?team=yang`
- **Team pyATS:** `http://YOUR-IP:8000/buzzer.html?team=pyats`

**Finding your laptop's IP:**
```bash
ifconfig | grep "inet " | grep -v 127.0.0.1
# Look for 192.168.x.x or 10.x.x.x
```

Convert these URLs to QR codes using any online QR generator (e.g., qr-code-generator.com), then print and place one at each team table.

### 4. Host Setup (Your Laptop/Projector)
Open the main game board:
```
http://localhost:8000/autocor_jeopardy.html
```

### 5. Team Setup (Participant Phones)
1. Each team member scans their table's QR code
2. They see their team name and icon
3. Anyone at the table can buzz in for the team

### 6. Game Flow
1. **Host clicks question tile** → buzzer auto-resets for all teams
2. **Host reads question** → teams race to buzz
3. **First buzz locks all others** → team name shows on host screen
4. **Host awards/deducts points** using +/- buttons
5. **Host clicks next question** → buzzer auto-resets (or use manual reset button)

## Team Configuration
- 🔴 **Team Ansible** - Red circle with "A"
- 🟣 **Team Terraform** - Purple square with "T"
- 🔵 **Team Python** - Blue/yellow circle with "P"
- 🟢 **Team YANG** - Green circle with tree icon
- 🟠 **Team pyATS** - Orange circle with test flask icon

## Game Sessions
Load different question sets via URL parameter:
- Session 1: `autocor_jeopardy.html?board=session_1`
- Session 2: `autocor_jeopardy.html?board=session_2`
- Session 3: `autocor_jeopardy.html?board=session_3`

Add your own sessions by creating JSON files in `/data/` folder.

## Files Overview
- **`autocor_jeopardy.html`** - Main game board (host display on projector)
- **`buzzer.html`** - Team buzzer page (accessed via QR codes with `?team=` parameter)
- **`test-buzzers.html`** - Pre-session testing page with MQTT debug console
- **`data/*.json`** - Question sets for different sessions

## Technical Details
- **MQTT Broker:** `wss://test.mosquitto.org:8081` (public, no auth required)
- **Topics:** 
  - `jeopardy/buzz` - buzzer presses from teams
  - `jeopardy/control` - reset/disable commands from host
- **No backend required** - pure client-side with MQTT over WSS (port 443)
- **Works on enterprise WiFi** - uses WebSocket Secure (port 443), rarely blocked
- **Team URLs:** Each team gets unique URL like `buzzer.html?team=ansible`

## Deployment Options

### Option 1: Local Server (Simple)
- Run Python server on your laptop
- Teams connect via your laptop's IP
- Requires all devices on same WiFi

### Option 2: GitHub Pages (Recommended for Cisco Live)
1. Push repo to GitHub
2. Settings → Pages → Enable (source: main branch)
3. Update QR codes with your GitHub Pages URL
4. No WiFi restrictions - works anywhere with internet

## Troubleshooting

**Buzzers not working?**
- Check 🟢 Live indicator on host screen (top right)
- Ensure phones show "🟢 Connected" status
- Test public MQTT broker: https://test.mosquitto.org

**Wrong team showing on phone?**
- Each QR code is team-specific
- Make sure correct QR code is at each table
- If wrong team, scan the correct QR code

**Can't access from phones (local server)?**
- Verify all devices on same WiFi
- Check laptop firewall allows port 8000
- Use GitHub Pages instead

**Fallback Mode:**
If MQTT fails, manual scoring still works - host clicks +/- buttons in question modal.

## Branch
Active development on: `alex-trebek-rip`

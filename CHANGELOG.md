# CCNP AUTOCOR Jeopardy - Branch: alex-trebek-rip

## Overview
Interactive Jeopardy game with real-time team buzzers for Cisco Live session [IBOCRT-2775].

## What's New in This Branch

### 🎮 Real-Time Buzzer System
- **Team Buzzers**: 5 teams can buzz in from their phones via MQTT
- **Unique URLs**: Each team gets their own QR code URL
- **Buzz Sound**: Fun electronic beep when teams buzz
- **Steal Mechanic**: Wrong answers allow other teams to buzz and steal

### 🎯 Smart Scoring
- **Quick Buttons**: ✓ CORRECT / ✗ WRONG for fast judging
- **Judge First**: Answer stays hidden until you decide
- **Auto-Reset**: Buzzers reset when you click next question
- **Manual Override**: Full control with +/- buttons for all teams

### 🔒 Private Answer Key
- **Separate Display**: `answer-key.html` for your private laptop
- **Password Protected**: `peaceD00d!`
- **Real-Time Sync**: Shows answer when you click question
- **Large Text**: Easy-to-read answer display

### 🎨 Cisco Live Branding
- Official Cisco Live 2025 logo
- Professional blue/white design
- Matches Cisco branding guidelines

### 📱 Mobile-Friendly
- Team buzzers work on any phone browser
- No app installation required
- Works on enterprise WiFi (uses MQTT over WSS port 443)

---

## Quick Start

### 1. Start Server
```bash
python3 -m http.server 8000
```

### 2. Update URLs for Testing

**For local testing (same machine):**
- Use `localhost:8000`

**For team testing (different devices on same WiFi):**
- Find your IP: `ipconfig getifaddr en0` (Mac) or `ipconfig` (Windows)
- Replace `localhost` with your IP (e.g., `192.168.1.100`)

### 3. Open Pages

**Main Game Board (projector):**
```
http://localhost:8000/autocor_jeopardy.html
```

**Answer Key (your private laptop):**
```
http://localhost:8000/answer-key.html
Password: peaceD00d!
```

**Team Buzzer URLs (convert to QR codes):**
```
http://localhost:8000/buzzer.html?team=ansible
http://localhost:8000/buzzer.html?team=terraform
http://localhost:8000/buzzer.html?team=python
http://localhost:8000/buzzer.html?team=yang
http://localhost:8000/buzzer.html?team=pyats
```

**Test Page (verify everything works):**
```
http://localhost:8000/test-buzzers.html
```

---

## Game Flow

1. **Setup**: Display main board on projector, open answer key on your laptop
2. **Click Question**: Question appears on main board, answer appears on your private screen
3. **Teams Buzz**: First team to hit buzzer is shown on screen
4. **You Judge**: Click ✓ CORRECT or ✗ WRONG (answer stays hidden from audience)
5. **Correct**: Answer reveals, points added, move to next question
6. **Wrong**: Points deducted, other teams can buzz and steal
7. **Close Modal**: Click X, click outside, or click "CLOSE QUESTION" button

---

## Files

- `autocor_jeopardy.html` - Main game board
- `buzzer.html` - Team buzzer page (accessed via QR codes)
- `answer-key.html` - Private answer display (password protected)
- `test-buzzers.html` - Pre-session testing page
- `data/*.json` - Question sets

---

## Team Configuration

- 🔴 **Team Ansible** - Red circle
- 🟣 **Team Terraform** - Purple square
- 🔵 **Team Python** - Blue/yellow circle
- 🟢 **Team YANG** - Green tree
- 🟠 **Team pyATS** - Orange flask

---

## Technical Details

- **MQTT Broker**: `wss://test.mosquitto.org:8081` (public, no auth)
- **Topics**:
  - `jeopardy/buzz` - Team buzzer presses
  - `jeopardy/control` - Reset/disable commands
  - `jeopardy/question` - Question/answer sync to answer key
- **No Backend Required**: Pure client-side with MQTT
- **Works on Enterprise WiFi**: Uses WebSocket Secure (port 443)

---

## Testing Checklist

- [ ] Main board loads and shows questions
- [ ] Answer key loads with password
- [ ] Test page shows all 5 team buzzers
- [ ] Buzz sound plays when testing
- [ ] Buzzer shows on main board when clicked
- [ ] Answer appears on answer key page
- [ ] Wrong answer locks out team, allows others to buzz
- [ ] Correct answer reveals and closes question
- [ ] Manual override buttons work

---

## Known Issues / Notes

- **First Click**: Click anywhere on main board first to unlock audio (browser security)
- **IP Changes**: If you reconnect to WiFi, IP address may change - update URLs
- **MQTT Public Broker**: Using public test broker - may have latency in busy environments

---

## Next Steps (Before Cisco Live)

1. **Deploy to GitHub Pages**: Permanent URLs, no IP address changes
2. **Generate QR Codes**: Use URLs above with any QR generator
3. **Print QR Codes**: One per team table
4. **Test on Venue WiFi**: Verify MQTT works on Cisco Live network
5. **Backup Plan**: Have mobile hotspot ready if venue WiFi blocks MQTT

---

## Questions?

This is a stopgap setup for local testing. Once deployed to GitHub Pages, all URLs will be permanent and work from anywhere.

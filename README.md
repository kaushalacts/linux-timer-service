# ⏰ Linux Timer Service

> A production-ready Linux service built from scratch that writes timestamps every 5 seconds. Demonstrating systemd mastery and real-world service management.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Linux](https://img.shields.io/badge/OS-Linux-blue.svg)](https://www.linux.org/)
[![systemd](https://img.shields.io/badge/Service-systemd-green.svg)](https://systemd.io/)
[![Status](https://img.shields.io/badge/Status-Production%20Ready-brightgreen.svg)](#)

---

## 🚀 **What This Project Does**

This is a **custom Linux service** I built to showcase:
- **Professional systemd service creation**
- **Real-world debugging and troubleshooting**
- **Production-ready automation**
- **Clean service management practices**

### ⚡ **Core Features**
- 🕒 **Automatic timestamp logging** every 5 seconds
- 🔄 **Auto-restart on failure** with configurable delays  
- 📊 **Full systemctl integration** (start, stop, status, logs)
- 🛡️ **Non-root execution** for security
- 📝 **Comprehensive logging** with cleanup handling
- 🚀 **Boot-time startup** capability

---

## 🎯 **Quick Start**

### **Clone & Deploy**
```bash
# Clone the repository
git clone https://github.com/YOUR_USERNAME/linux-timer-service.git
cd linux-timer-service

# Make installer executable
chmod +x install.sh

# Install the service
sudo ./install.sh

# Start the service
sudo systemctl start timer
sudo systemctl enable timer  # Auto-start on boot

# Check it's running
sudo systemctl status timer
```

### **Verify It's Working**
```bash
# Watch live output
tail -f ~/timer-output.txt

# Check service logs
sudo journalctl -u timer -f
```

---

## 📂 **Project Structure**

```
linux-timer-service/
├── 📜 timer-script.sh          # Core service script
├── ⚙️  timer.service           # systemd service definition  
├── 🛠️ install.sh              # Automated installer
├── 🗑️ uninstall.sh            # Clean removal script
├── 📋 README.md               # This file
└── 🖼️ screenshots/            # Demo images
    ├── service-status.png
    └── log-output.png
```

---

## 🔧 **Technical Implementation**

### **Service Architecture**
- **Language:** Pure Bash (no external dependencies)
- **Service Type:** Simple systemd service
- **Execution User:** Non-privileged user account
- **Output Location:** User home directory (`~/timer-output.txt`)
- **Restart Policy:** Always restart with 3-second delay

### **Key Components**

#### 🔹 **Service Script** (`timer-script.sh`)
```bash
#!/bin/bash
# Handles signal trapping, clean shutdowns, and continuous operation
```

#### 🔹 **Service Definition** (`timer.service`)
```ini
[Unit]
Description=Timer Service - Production timestamp logger
After=network.target

[Service]
Type=simple
User=laborant
ExecStart=/usr/local/bin/timer-script.sh
Restart=always
RestartSec=3
Environment=HOME=/home/laborant

[Install]
WantedBy=multi-user.target
```

---

## 📊 **Real Performance Metrics**

**During Development & Testing:**
- ✅ **89 restart attempts** during debugging (now resolved)
- ✅ **Memory usage:** ~720KB 
- ✅ **CPU impact:** Minimal (12ms)
- ✅ **Uptime:** Tested for 24+ hours continuous operation
- ✅ **File I/O:** Clean append operations, no corruption

**Production Stats:**
```bash
● timer.service - Timer Service - Production timestamp logger
     Active: active (running) since Sun 2025-08-10 12:49:25 UTC; 2h 15m ago
   Main PID: 1677 (timer-script.sh)
      Tasks: 2 (limit: 9535)
     Memory: 720.0K
        CPU: 45ms
```

---

## 🛡️ **Security & Best Practices**

### **Security Features Implemented:**
- 🔒 **Non-root execution** - Runs under regular user account
- 🛡️ **Signal handling** - Graceful shutdown on SIGTERM/SIGINT
- 📝 **Controlled file access** - Writes only to user home directory
- 🚫 **No network access** - Local operation only
- ✅ **Proper permissions** - Scripts: 755, Service files: 644

### **Production Considerations:**
- **Log rotation** - Consider implementing for long-term deployment
- **Monitoring** - Integrate with your monitoring stack
- **Backup** - Output file location is configurable
- **Resource limits** - Can be added to service file if needed

---

## 🔍 **Troubleshooting & Debugging**

### **Common Issues I Solved:**

#### ❌ **"Failed to determine group credentials"**
```bash
# Problem: Incorrect User/Group configuration
# Solution: Fixed service file with proper user settings
User=laborant
# Removed problematic Group= line
```

#### ❌ **Permission Denied**
```bash
# Problem: Script not executable
# Solution: 
sudo chmod +x /usr/local/bin/timer-script.sh
```

#### ❌ **Service Won't Start**
```bash
# Debug with detailed logs:
sudo journalctl -u timer -f
```

### **Monitoring Commands**
```bash
# Service status
sudo systemctl status timer

# Live logs
sudo journalctl -u timer -f

# Resource usage
top -p $(pgrep -f timer-script)

# Output file status
ls -la ~/timer-output.txt
wc -l ~/timer-output.txt  # Line count
```

---

## ⚙️ **Configuration & Customization**

### **Easy Modifications:**

#### **Change Timer Interval**
Edit `timer-script.sh`:
```bash
sleep 5  # Change to desired seconds
```

#### **Change Output Location**
Edit `timer-script.sh`:
```bash
OUTPUT_FILE="$HOME/timer-output.txt"  # Modify path
```

#### **Auto-start Configuration**
```bash
# Enable auto-start on boot
sudo systemctl enable timer

# Disable auto-start
sudo systemctl disable timer
```

---

## 📈 **Future Enhancements**

### **Planned Features:**
- [ ] **Configuration file support** - External config for intervals
- [ ] **Log rotation integration** - Automatic log management
- [ ] **Metrics endpoint** - Prometheus-compatible metrics
- [ ] **Multi-format output** - JSON, CSV support
- [ ] **Remote logging** - Syslog integration
- [ ] **Health checks** - Built-in service health monitoring

### **Advanced Use Cases:**
- 🔍 **System monitoring** - Extend for CPU/memory tracking
- 📊 **Application logging** - Customize for app-specific metrics  
- 🚨 **Alert triggers** - Add threshold-based notifications
- 🔄 **Backup automation** - Schedule automated backups
- 📡 **API health monitoring** - Monitor external services

---

## 🚀 **Installation Options**

### **Option 1: Automated Install**
```bash
curl -sSL https://raw.githubusercontent.com/YOUR_USERNAME/linux-timer-service/main/install.sh | sudo bash
```

### **Option 2: Manual Install**
```bash
# 1. Copy script
sudo cp timer-script.sh /usr/local/bin/
sudo chmod +x /usr/local/bin/timer-script.sh

# 2. Install service
sudo cp timer.service /etc/systemd/system/
sudo systemctl daemon-reload

# 3. Start and enable
sudo systemctl start timer
sudo systemctl enable timer
```

### **Option 3: User Service (No sudo required)**
```bash
# Create user service directory
mkdir -p ~/.config/systemd/user

# Copy service file
cp timer-user.service ~/.config/systemd/user/timer.service

# Start user service
systemctl --user daemon-reload
systemctl --user start timer
systemctl --user enable timer
```

---

## 📸 **Demo Screenshots**

### **Service Running Successfully**
![Service Status](screenshots/service-status.png)

### **Live Log Output**
![Log Output](screenshots/log-output.png)

---

## 🎯 **Development Journey**

This project taught me:
- ✅ **systemd service architecture**
- ✅ **Real debugging skills** (89 failed restarts!)
- ✅ **Production troubleshooting**
- ✅ **Linux system administration**
- ✅ **Service lifecycle management**

**Key Learning:** The difference between reading about Linux services and actually building one is huge. The debugging experience alone was worth the entire project.

---

## 🤝 **Contributing**

Found a bug? Want to add a feature? Contributions welcome!

1. **Fork** the repository
2. **Create** a feature branch: `git checkout -b feature-name`
3. **Commit** changes: `git commit -m 'Add feature'`
4. **Push** to branch: `git push origin feature-name`  
5. **Open** a Pull Request

---

## 📝 **License**

This project is licensed under the **MIT License** - see the [LICENSE](LICENSE) file for details.

---

## 🔗 **Connect With Me**

- 💼 **LinkedIn:** [Your LinkedIn Profile]
- 🐦 **Twitter:** [@YourTwitter] 
- 📝 **Blog:** [Your Blog URL]
- 💻 **GitHub:** [@YourGitHub]

---

## ⭐ **Support This Project**

If this project helped you learn something new:
- ⭐ **Star this repository**
- 🍴 **Fork it** for your own experiments
- 📢 **Share it** with fellow developers
- 💬 **Open issues** for questions or suggestions

---

**Built with ❤️ for the Linux community**

---

## 📚 **Related Projects**

- [Advanced System Monitor Service](https://github.com/yourname/system-monitor-service)
- [Log Rotation Automation](https://github.com/yourname/log-rotation-service)
- [API Health Checker Service](https://github.com/yourname/api-health-service)

---

*Last updated: August 2025*

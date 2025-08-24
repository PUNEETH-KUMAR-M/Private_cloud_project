# 🏠 CasaOS on Raspberry Pi OS Lite + WireGuard + Android Access

[![Raspberry Pi](https://img.shields.io/badge/Raspberry%20Pi-4-red?style=for-the-badge&logo=raspberry-pi)](https://www.raspberrypi.org/)
[![CasaOS](https://img.shields.io/badge/CasaOS-0.4.0+-blue?style=for-the-badge)](https://casaos.io/)
[![WireGuard](https://img.shields.io/badge/WireGuard-VPN-green?style=for-the-badge)](https://www.wireguard.com/)
[![Android](https://img.shields.io/badge/Android-Mobile-orange?style=for-the-badge&logo=android)](https://www.android.com/)

> **A practical, copy-paste friendly walkthrough to turn a Raspberry Pi into a mobile-friendly NAS and securely access/stream it from Android.**

## 📋 Table of Contents

- [Overview](#overview)
- [Prerequisites](#prerequisites)
- [Quick Start](#quick-start)
- [Detailed Setup](#detailed-setup)
- [Configuration](#configuration)
- [Troubleshooting](#troubleshooting)
- [Security & Maintenance](#security--maintenance)
- [Useful Commands](#useful-commands)
- [References](#references)

## 🎯 Overview

This project transforms your Raspberry Pi into a powerful, secure personal cloud server using CasaOS. With WireGuard VPN, you can access your files and stream media from anywhere while maintaining enterprise-grade security.

### ✨ Key Features

- **🏠 CasaOS Dashboard** - Beautiful, intuitive web interface for managing your personal cloud
- **🔒 WireGuard VPN** - Secure remote access without exposing services to the internet
- **📱 Android Integration** - Seamless mobile access to files and media
- **🎬 Media Streaming** - Jellyfin server for movies, music, and photos
- **☁️ File Sync** - Nextcloud for personal cloud storage and synchronization
- **💾 Reliable Storage** - External drive mounting with proper permissions

## 🛠️ Prerequisites

### Hardware Requirements
- **Raspberry Pi 4** (2GB/4GB recommended) or later
- **microSD card** 16–32GB (for OS)
- **External USB HDD/SSD** (USB 3 recommended for better performance)
- **Official 5V power supply** (3A recommended)
- **Computer** to flash SD card, or headless via SSH
- **Android phone** (WireGuard app, file manager, media player)

### Network Requirements
- **Ethernet connection** (recommended for NAS stability)
- **WiFi access** (for initial setup and mobile access)
- **Router access** (for DHCP reservation)

## 🚀 Quick Start

### 1. Flash Raspberry Pi OS Lite
```bash
# Download and flash using Raspberry Pi Imager
# Enable SSH and WiFi (headless setup)
# Insert SD card and power on
```

### 2. Install CasaOS (One-liner!)
```bash
curl -fsSL https://get.casaos.io | sudo bash
```

### 3. Access Dashboard
Open your browser and visit: `http://<PI-IP>/`

### 4. Install Essential Apps
- **Jellyfin** - Media streaming server
- **Nextcloud** - Personal cloud storage
- **WireGuard** - VPN server

## 📖 Detailed Setup

### Step 1: Initial Pi Setup

```bash
# SSH into your Pi (default: pi@raspberry)
ssh pi@<PI-IP>

# Change default password immediately
passwd

# Update system
sudo apt update && sudo apt full-upgrade -y

# Optional: Configure system
sudo raspi-config
# Set Locale, Timezone, Hostname, enable SSH
```

### Step 2: Install CasaOS

```bash
# One-liner installation
curl -fsSL https://get.casaos.io | sudo bash

# Wait for installation to complete
# Access dashboard at http://<PI-IP>/
# Create your CasaOS user account
```

### Step 3: Prepare External Storage

```bash
# Identify your external drive
lsblk -f

# Create mount point and mount
sudo mkdir -p /mnt/storage
sudo mount /dev/sda1 /mnt/storage

# Get UUID for persistent mounting
sudo blkid /dev/sda1

# Add to /etc/fstab for auto-mounting
sudo nano /etc/fstab

# Add this line (replace UUID with your actual UUID):
# UUID=your-uuid-here /mnt/storage ext4 defaults,nofail 0 2

# Set proper permissions
sudo chown -R 1000:1000 /mnt/storage
```

### Step 4: Configure CasaOS Apps

1. **Jellyfin Setup**
   - Install from CasaOS App Store
   - Set volume mapping: `/mnt/storage/media` → `/config`
   - Access at `http://<PI-IP>:8096`

2. **Nextcloud Setup**
   - Install from CasaOS App Store
   - Set volume mapping: `/mnt/storage/nextcloud` → `/data`
   - Access at `http://<PI-IP>:8080`

3. **WireGuard Setup**
   - Install from CasaOS App Store
   - Create new client profile (Android)
   - Export QR code for mobile setup

### Step 5: WireGuard Configuration

#### Option A: CasaOS WireGuard App
```bash
# Install WireGuard app from CasaOS App Store
# Create new client profile
# Export QR code
# Scan with Android WireGuard app
```

#### Option B: PiVPN (Alternative)
```bash
# Install PiVPN
curl -L https://install.pivpn.io | bash

# Choose WireGuard during installation
# Create client
pivpn add

# Show QR code
pivpn -qr <client-name>
```

### Step 6: Android Setup

1. **Install Required Apps**
   - [WireGuard](https://play.google.com/store/apps/details?id=com.wireguard.android)
   - [Solid Explorer](https://play.google.com/store/apps/details?id=pl.solidexplorer2) or [CX File Explorer](https://play.google.com/store/apps/details?id=com.cxinventor.file.explorer)
   - [Jellyfin](https://play.google.com/store/apps/details?id=org.jellyfin.mobile)
   - [Nextcloud](https://play.google.com/store/apps/details?id=com.nextcloud.client)

2. **Connect to VPN**
   - Scan QR code from WireGuard setup
   - Enable the profile
   - Verify connection to your LAN

3. **Access Files**
   - **SMB Access**: Add SMB location in file explorer
     - Host: `<PI-IP>`
     - Username/Password: Your Pi credentials
   - **Nextcloud**: Login with your CasaOS/Nextcloud credentials
   - **Jellyfin**: Access via app or browser at `<PI-IP>:8096`

## ⚙️ Configuration

### Static IP Setup (Recommended)
```bash
# Option 1: Router DHCP Reservation (Preferred)
# Set static IP in your router's admin panel

# Option 2: Static IP on Pi
sudo nano /etc/dhcpcd.conf

# Add at the end:
# interface eth0
# static ip_address=192.168.1.100/24
# static routers=192.168.1.1
# static domain_name_servers=8.8.8.8 8.8.4.4
```

### Storage Permissions
```bash
# If apps can't access storage, check permissions
sudo chown -R 1000:1000 /mnt/storage
sudo chmod -R 755 /mnt/storage

# For debugging (temporary)
sudo chmod -R 777 /mnt/storage
```

## 🔧 Troubleshooting

### Common Issues & Solutions

#### Drive Not Visible in Apps
```bash
# Check if drive is mounted
lsblk -f

# Verify mount point
df -h

# Check permissions
ls -la /mnt/storage

# Restart container after fixing permissions
```

#### Jellyfin Streams Not Found
- Verify container volume mapping
- Check network mode in app settings
- Ensure media folders are properly mapped

#### WireGuard Connection Issues
```bash
# Check Pi time (NTP)
sudo timedatectl status

# Verify WireGuard service
sudo systemctl status wg-quick@wg0

# Check firewall rules
sudo iptables -L
```

#### Performance Issues
```bash
# Check CPU/memory usage
htop

# Monitor disk I/O
iotop

# Check network speed
speedtest-cli
```

## 🔒 Security & Maintenance

### Security Checklist
- [ ] Change all default passwords
- [ ] Keep Pi and CasaOS updated
- [ ] Use strong, unique passwords
- [ ] Enable firewall rules
- [ ] Regular security updates

### Maintenance Commands
```bash
# System updates
sudo apt update && sudo apt upgrade -y

# CasaOS updates (via dashboard)
# App updates (via CasaOS App Store)

# Backup critical data
sudo rsync -av /mnt/storage/ /backup/storage/

# Monitor system health
sudo systemctl status casaos
sudo journalctl -u casaos -f
```

### Backup Strategy
- **Nextcloud data**: Regular backups to external drive
- **Configuration files**: Backup `/etc/casaos/` and app configs
- **Media files**: Sync to cloud storage or external drives
- **System image**: Create SD card backup monthly

## 📱 Useful Commands

### Quick Reference
```bash
# System management
sudo apt update && sudo apt full-upgrade -y
sudo systemctl restart casaos
sudo systemctl status casaos

# Storage management
lsblk -f                    # List block devices
df -h                       # Show disk usage
sudo blkid /dev/sda1       # Get drive UUID
sudo mount /dev/sda1 /mnt/storage  # Mount drive

# Network management
ip addr show                # Show network interfaces
sudo systemctl status wg-quick@wg0  # WireGuard status
sudo iptables -L            # Show firewall rules

# CasaOS management
curl -fsSL https://get.casaos.io | sudo bash  # Install
sudo systemctl restart casaos                  # Restart service
```

### PiVPN Commands (if using)
```bash
pivpn add                   # Add new client
pivpn list                  # List all clients
pivpn -qr <client-name>    # Show QR code
pivpn revoke <client-name> # Remove client
```

## 📚 References

### Official Documentation
- [CasaOS Documentation](https://docs.casaos.io/) - Official CasaOS docs and Wiki
- [Jellyfin Documentation](https://jellyfin.org/docs/) - Media server setup and configuration
- [Nextcloud Documentation](https://docs.nextcloud.com/) - Personal cloud platform docs
- [WireGuard Documentation](https://www.wireguard.com/quickstart/) - VPN protocol documentation

### Community Resources
- [CasaOS Community](https://github.com/IceWhaleTech/CasaOS) - GitHub repository and discussions
- [PiVPN Documentation](https://docs.pivpn.io/) - Alternative WireGuard setup
- [Raspberry Pi Forums](https://forums.raspberrypi.com/) - Community support and tips

### Related Guides
- [Mounting External Drives on Raspberry Pi](https://www.raspberrypi.org/documentation/configuration/external-storage.md)
- [Raspberry Pi Network Configuration](https://www.raspberrypi.org/documentation/configuration/tcpip/)
- [Docker on Raspberry Pi](https://docs.docker.com/engine/install/debian/)

## 🤝 Contributing

This project is open for contributions! Feel free to:
- Report issues
- Suggest improvements
- Submit pull requests
- Share your setup experiences

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 👨‍💻 Author

**Puneeth Kumar M** - Engineering student, tech enthusiast, and passionate builder.

- GitHub: [@PUNEETH-KUMAR-M](https://github.com/PUNEETH-KUMAR-M)
- Project: [Private Cloud Project](https://github.com/PUNEETH-KUMAR-M/Private_cloud_project)

---

<div align="center">

**⭐ Star this repository if it helped you! ⭐**

*Built with ❤️ for the Raspberry Pi community*

</div>
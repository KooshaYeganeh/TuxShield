# TuxShield - Complete Linux Security Scanner

<div align="center">

![TuxShield Logo](https://img.shields.io/badge/TuxShield-Security%20Scanner-blue)
![Version](https://img.shields.io/badge/version-1.0.0-green)
![License](https://img.shields.io/badge/license-GPLv2-red)
![Linux](https://img.shields.io/badge/Linux-Supported-brightgreen)

**Your All-in-One Security Solution for Linux Devices**

[Installation](#installation) • [Quick Start](#quick-start) • [Tools](#security-tools) • [Documentation](#documentation)

</div>

---

## 📋 Table of Contents

- [Overview](#overview)
- [Security Tools](#security-tools)
- [Installation](#installation)
- [Quick Start](#quick-start)
- [Build from Source](#build-from-source)
- [Configuration](#configuration)
- [Usage Examples](#usage-examples)
- [Logs & Reports](#logs--reports)
- [Uninstallation](#uninstallation)
- [FAQ](#faq)
- [License](#license)

---

## 🛡️ Overview

**TuxShield** is a comprehensive security scanning suite that integrates **7 powerful security tools** into one easy-to-use interface. It's designed for Linux systems to detect malware, rootkits, vulnerabilities, and security misconfigurations.

### Features

- ✅ **7-in-1 Security Tool Integration**
- ✅ **Automated Daily Updates** (systemd timer)
- ✅ **Real-time Scan Progress** with live output
- ✅ **Comprehensive Logging** to `/var/log/tuxshield/`
- ✅ **Cron Jobs** for automated scanning
- ✅ **Low Resource Usage** (scans only when you run it)
- ✅ **Cross-Distribution Support** (Debian/Ubuntu, RHEL/Fedora, openSUSE)

---

## 🔧 Security Tools

| # | Tool | Purpose | Installation Method |
|---|------|---------|---------------------|
| 1 | **ClamAV** | Antivirus & Malware Detection | `sudo apt install clamav clamav-daemon` |
| 2 | **Linux Malware Detect (LMD)** | Linux-specific Malware Scanner | [Manual Install](#linux-malware-detection-lmd) |
| 3 | **Rkhunter** | Rootkit Hunter | `sudo apt install rkhunter` |
| 4 | **Chkrootkit** | Rootkit Detector | `sudo apt install chkrootkit` |
| 5 | **Lynis** | Security Auditing Tool | `sudo apt install lynis` |
| 6 | **Nuclei** | Vulnerability Scanner | [Build from Source](#nuclei-installation) |
| 7 | **SemanticsAV** | AI-Powered Malware Scanner | [Build from Source](#semanticsav-installation) |

---

## 📥 Installation

### Method 1: Debian Package (Recommended)

```bash
# Download the .deb package
wget https://github.com/tuxshield/tuxshield/releases/download/v1.0.0/tuxshield_1.0.0.deb

# Install the package
sudo dpkg -i tuxshield_1.0.0.deb

# Fix any dependency issues
sudo apt-get install -f

# Verify installation
tuxshield --status
```

### Method 2: Direct Script Installation

```bash
# Clone the repository
git clone https://github.com/tuxshield/tuxshield.git
cd tuxshield

# Make script executable
chmod +x tuxshield

# Install all tools automatically
sudo ./tuxshield --install

# Verify installation
./tuxshield --status
```

### Method 3: Manual Tool Installation

If you prefer to install tools manually or need specific versions, follow these guides:

#### Linux Malware Detection (LMD)

```bash
# Download and run the official installer
wget https://github.com/rfxn/linux-malware-detect/raw/master/install.sh
sudo bash install.sh

# Verify installation
sudo maldet --version
```

**More Information:** [Linux Malware Detect GitHub](https://github.com/rfxn/linux-malware-detect)

#### Nuclei Installation

```bash
# Option 1: Build from source (recommended)
git clone https://github.com/projectdiscovery/nuclei.git
cd nuclei/cmd/nuclei
go build
sudo mv nuclei /usr/local/bin/

# Download templates
git clone https://github.com/projectdiscovery/nuclei-templates.git ~/nuclei-templates
cd ~/nuclei-templates
git pull

# Option 2: Download pre-built binary
wget https://github.com/projectdiscovery/nuclei/releases/latest/download/nuclei-linux-amd64.zip
unzip nuclei-linux-amd64.zip
sudo mv nuclei /usr/local/bin/
```

**More Information:** [Nuclei Documentation](https://docs.projectdiscovery.io/opensource/nuclei/install#github)

#### SemanticsAV Installation

```bash
# Clone repository
git clone https://github.com/metaforensics-ai/semantics-av-cli.git
cd semantics-av-cli

# Build from source
mkdir build && cd build
cmake -DCMAKE_BUILD_TYPE=Release ..
make -j$(nproc)

# Option A: System-wide installation (requires root)
sudo make install
sudo /usr/local/share/semantics-av/post_install.sh

# Option B: User-local installation (no root required)
cmake -DCMAKE_INSTALL_PREFIX=~/.local ..
make install
~/.local/share/semantics-av/post_install_user.sh
export PATH="$HOME/.local/bin:$PATH"

# Initialize and update
semantics-av config init --defaults
semantics-av update
```

**More Information:** [SemanticsAV GitHub](https://github.com/metaforensics-ai/semantics-av-cli)

#### ClamAV, Rkhunter, Chkrootkit, Lynis

```bash
# Debian/Ubuntu
sudo apt update
sudo apt install -y clamav clamav-daemon rkhunter chkrootkit lynis

# RHEL/Fedora
sudo dnf install -y clamav clamav-update rkhunter chkrootkit lynis

# Update ClamAV signatures
sudo freshclam
```

---

## 🚀 Quick Start

### Basic Commands

```bash
# Show all available commands
tuxshield --help

# Check installation status
tuxshield --status

# Run complete security audit (ALL 7 tools)
sudo tuxshield --full

# Quick daily scan (ClamAV + Rkhunter + Chkrootkit)
sudo tuxshield --quick

# Scan specific directory
sudo tuxshield --scan ~/Downloads
```

### Individual Tool Scans

```bash
# ClamAV antivirus scan
sudo tuxshield --clamav /home/user/Downloads

# Linux Malware Detect scan
sudo tuxshield --maldet /var/www

# Rootkit checks
sudo tuxshield --rkhunter
sudo tuxshield --chkrootkit

# Security audit
sudo tuxshield --lynis

# Vulnerability scan (any website/IP)
sudo tuxshield --nuclei https://example.com

# AI-powered malware scan
sudo tuxshield --semantics ~/Downloads
```

### Maintenance Commands

```bash
# Update all tools and signatures
sudo tuxshield --update

# Install all tools (first-time setup)
sudo tuxshield --install

# Remove TuxShield
sudo dpkg --purge tuxshield
```

---

## 🏗️ Build from Source

### Build Debian Package

```bash
# Clone repository
git clone https://github.com/tuxshield/tuxshield.git
cd tuxshield

# Make build script executable
chmod +x build.sh

# Build the .deb package
./build.sh

# Install the generated package
sudo dpkg -i tuxshield_1.0.0.deb
```

### Manual Installation Without Package

```bash
# Copy script to system path
sudo cp tuxshield /usr/local/bin/
sudo chmod +x /usr/local/bin/tuxshield

# Create required directories
sudo mkdir -p /var/log/tuxshield /etc/tuxshield

# Install tools manually (see tool-specific guides above)
```

---

## ⚙️ Configuration

### Configuration File Location

```
/etc/tuxshield/tuxshield.conf
```

### Example Configuration

```toml
# TuxShield Configuration
log_level = "info"
scan_threads = 4
max_file_size = "100MB"
exclude_dirs = ["/proc", "/sys", "/dev", "/run"]

[clamav]
enabled = true
max_files = 0
max_scansize = 0

[maldet]
enabled = true
scan_heuristic = true

[rkhunter]
enabled = true
auto_update = true

[chkrootkit]
enabled = true

[lynis]
enabled = true

[nuclei]
enabled = true
severity = ["low", "medium", "high", "critical"]

[semanticsav]
enabled = true
recursive = true
```

### Cron Jobs

TuxShield automatically installs the following cron jobs:

- **Daily**: Update all signatures (`/etc/cron.daily/tuxshield-update`)
- **Weekly**: Full system scan (`/etc/cron.weekly/tuxshield-scan`)
- **Systemd Timer**: Automated daily updates

---

## 📝 Usage Examples

### Real-World Scenarios

#### 1. **Daily Security Check (Quick Scan)**
```bash
sudo tuxshield --quick
```
*Use this daily to check for new threats quickly*

#### 2. **Complete System Audit**
```bash
sudo tuxshield --full
```
*Run this weekly or monthly for comprehensive security*

#### 3. **Scan Downloads Before Opening**
```bash
sudo tuxshield --semantics ~/Downloads
sudo tuxshield --clamav ~/Downloads
```
*Always scan downloaded files before opening*

#### 4. **Web Server Security**
```bash
# Scan web directory
sudo tuxshield --scan /var/www/html

# Check for vulnerabilities
sudo tuxshield --nuclei https://yourwebsite.com
```

#### 5. **Malware Investigation**
```bash
# Use all tools on suspicious directory
sudo tuxshield --scan /suspicious/path
```

---

## 📊 Logs & Reports

### Log Locations

| Tool | Log Location |
|------|--------------|
| All Scans | `/var/log/tuxshield/full_scan_*/` |
| Quick Scans | `/var/log/tuxshield/quick_scan_*/` |
| ClamAV | `/var/log/tuxshield/*/clamav.log` |
| Maldet | `/var/log/tuxshield/*/maldet.log` |
| Rkhunter | `/var/log/tuxshield/*/rkhunter.log` |
| Chkrootkit | `/var/log/tuxshield/*/chkrootkit.log` |
| Lynis | `/var/log/tuxshield/*/lynis.log` |
| Nuclei | `/var/log/tuxshield/*/nuclei.log` |
| SemanticsAV | `/var/log/tuxshield/*/semanticsav.log` |
| Main Log | `/var/log/tuxshield.log` |

### Viewing Reports

```bash
# View latest scan results
ls -lt /var/log/tuxshield/ | head -5

# Check for infected files
grep -r "FOUND" /var/log/tuxshield/*/clamav.log

# Check rootkit warnings
grep -r "Warning" /var/log/tuxshield/*/rkhunter.log
```

---

## 🗑️ Uninstallation

### Remove Debian Package
```bash
sudo dpkg --purge tuxshield
```

### Remove All Tools (Manual)
```bash
# Remove TuxShield script
sudo rm -f /usr/local/bin/tuxshield

# Remove configuration and logs
sudo rm -rf /etc/tuxshield /var/log/tuxshield

# Remove installed tools (if no longer needed)
sudo apt remove --purge clamav clamav-daemon rkhunter chkrootkit lynis
sudo rm -rf /usr/local/maldetect
sudo rm -f /usr/local/bin/nuclei
sudo rm -rf ~/nuclei-templates ~/.local/bin/semantics-av ~/.local/share/semantics-av
```

---

## ❓ FAQ

### Q: Do I need to install all tools manually?
**A:** No! Run `sudo tuxshield --install` and the script will automatically install everything.

### Q: How long does a full scan take?
**A:** Depending on disk size:
- Quick scan: 2-5 minutes
- Full scan: 30 minutes to several hours

### Q: Can I schedule automatic scans?
**A:** Yes! TuxShield installs systemd timers for daily updates. Add custom cron jobs:
```bash
# Weekly full scan every Sunday at 2 AM
0 2 * * 0 /usr/local/bin/tuxshield --full
```

### Q: Does TuxShield affect system performance?
**A:** Scans only run when you execute them. The tool itself uses minimal resources when idle.

### Q: Can I scan remote servers?
**A:** TuxShield is designed for local scanning. Use `--nuclei` for remote vulnerability scanning.

### Q: How to update only specific tools?
**A:** Use individual commands:
```bash
sudo freshclam              # Update ClamAV
sudo maldet --update        # Update Maldet
sudo rkhunter --update      # Update Rkhunter
semantics-av update         # Update SemanticsAV
```

---

## 📚 Documentation

### Official Documentation Links

- [ClamAV Documentation](https://www.clamav.net/documents)
- [Linux Malware Detect](https://www.rfxn.com/projects/linux-malware-detect/)
- [Rkhunter Documentation](http://rkhunter.sourceforge.net/)
- [Lynis Documentation](https://cisofy.com/documentation/lynis/)
- [Nuclei Documentation](https://docs.projectdiscovery.io/opensource/nuclei/install#github)
- [SemanticsAV Documentation](https://github.com/metaforensics-ai/semantics-av-cli)

### Community & Support

- **GitHub Issues**: [Report bugs or request features](https://github.com/tuxshield/tuxshield/issues)
- **Security Advisories**: [Subscribe to updates](https://github.com/tuxshield/tuxshield/security)

---

## 📄 License

TuxShield is released under the **GNU General Public License v2.0**.

```
This program is free software; you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation; either version 2 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the
GNU General Public License for more details.
```

---

## 🙏 Acknowledgments

- **R-fx Networks** for Linux Malware Detect
- **ProjectDiscovery** for Nuclei
- **MetaForensics AI** for SemanticsAV
- **Cisco** for ClamAV
- **CISOfy** for Lynis
- All open-source contributors

---

<div align="center">

**Made with ❤️ for the Linux Security Community**

[Report Bug](https://github.com/tuxshield/tuxshield/issues) • [Request Feature](https://github.com/tuxshield/tuxshield/issues) • [Star on GitHub](https://github.com/tuxshield/tuxshield)

</div>
# **🔒 SA - Security Scanner**

**Easily scan your openSUSE system with multiple security tools using a single command!**  
![SUSE](https://en.opensuse.org/images/f/f2/Button-laptop-colour.png)

## 🛠️ **Included Tools**

1. **🛡️ ClamAV** - Antivirus software to detect malicious threats.
2. **🔍 RKHunter** - Rootkit detection tool.
3. **🦠 LMD (Linux Malware Detect)** - Detect and remove malware.
4. **🔒 Chkrootkit** - Another tool for rootkit detection.
5. **📋 Lynis** - Security auditing tool for Linux.

## 📦 **Installation**

To install the `sa` tool, run the following commands:

```bash
cd /tmp && wget https://github.com/KooshaYeganeh/sa/archive/refs/heads/main.zip && unzip main.zip && [ -d "/opt/script" ] && echo "Directory exists" || sudo mkdir /opt/script && cd sa-main && sudo mv sa /opt/script && echo "sa Installed [ OK ]" && cd && echo "Note : sa Tool is currently in /opt/script Directory"
```

> **Note:** On the first run, the script will automatically check and install any missing software.

> **Optional:** If you prefer not to scan all devices with Ansible, you can install the RAM file on your system:

```bash
sudo rpm -ivh sa-1.0.0.noarch.rpm
```

## 🚀 **Usage**

**Scan Everything:**

```bash
sa -a
```

Or

```bash
sa --all
```

**Update Tools:**

```bash
sa -u
```

Or

```bash
sa --update
```

**Scan All Hosts:**

```bash
sa --hosts
```

**Help:**

```bash
sa -h
```

Or

```bash
sa --help
```



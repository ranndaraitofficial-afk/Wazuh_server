# Wazuh SIEM Lab Setup

## Overview

This project demonstrates how to deploy a complete Wazuh SIEM lab environment using Ubuntu Server and pfSense.

Wazuh provides:
- SIEM monitoring
- Intrusion detection
- Log analysis
- Vulnerability detection
- File integrity monitoring
- Endpoint monitoring

---

# System Requirements

| Component | Minimum |
|---|---|
| CPU | 4 Cores |
| RAM | 8 GB |
| Storage | 50 GB |
| OS | Ubuntu Server 22.04 LTS |

---

# Update Ubuntu

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install curl unzip wget -y
```

---

# Configure Hostname

```bash
sudo hostnamectl set-hostname wazuh-server
hostname
```

---

# Download Wazuh Installer

```bash
curl -sO https://packages.wazuh.com/4.8/wazuh-install.sh
chmod +x wazuh-install.sh
```

---

# Install Wazuh All-in-One

```bash
sudo ./wazuh-install.sh -a
```

This installs:
- Wazuh Manager
- Wazuh Indexer
- Wazuh Dashboard

---

# Access Dashboard

Open browser:

```text
https://YOUR_SERVER_IP
```

Example:

```text
https://192.168.1.100
```

Default username:

```text
admin
```

Password is generated during installation.

---

# Check Wazuh Services

```bash
sudo systemctl status wazuh-manager
sudo systemctl status wazuh-indexer
sudo systemctl status wazuh-dashboard
```

Enable startup:

```bash
sudo systemctl enable wazuh-manager
sudo systemctl enable wazuh-indexer
sudo systemctl enable wazuh-dashboard
```

---

# Configure Firewall

```bash
sudo ufw allow 443/tcp
sudo ufw allow 1514/tcp
sudo ufw allow 1515/tcp
sudo ufw allow 55000/tcp
sudo ufw enable
```

---

# Important Ports

| Port | Purpose |
|---|---|
| 443 | Web Dashboard |
| 1514 | Agent Communication |
| 1515 | Agent Registration |
| 55000 | Wazuh API |

---

# Install Wazuh Agent

## Ubuntu Agent Installation

```bash
curl -s https://packages.wazuh.com/4.x/apt/install.sh | sudo bash
```

Edit configuration:

```bash
sudo nano /var/ossec/etc/ossec.conf
```

Find:

```xml
<address>MANAGER_IP</address>
```

Replace with your Wazuh server IP.

Restart service:

```bash
sudo systemctl restart wazuh-agent
sudo systemctl enable wazuh-agent
```

---

# Register Agent

## On Wazuh Server

```bash
sudo /var/ossec/bin/manage_agents
```

### Add Agent
- Press `A`
- Enter agent name
- Enter client IP

### Export Key
- Press `E`

---

## On Client Machine

```bash
sudo /var/ossec/bin/manage_agents
```

### Import Key
- Press `I`

Restart agent:

```bash
sudo systemctl restart wazuh-agent
```

---

# Verify Connection

Dashboard:

```text
Endpoints → Agents
```

You should see:
- Agent online
- Security events
- System logs

---

# Recommended Lab Topology

```text
Internet
   |
pfSense
   |
Core Switch
   |
--------------------------------
|              |               |
Wazuh      Windows Server    Ubuntu
Server         DC            Server
```

---

# Repository Structure

```text
wazuh-lab/
├── README.md
├── screenshots/
├── configs/
├── diagrams/
└── scripts/
```

---

# Useful Resources

- Official Documentation:
  https://documentation.wazuh.com/

- GitHub Repository:
  https://github.com/wazuh/wazuh

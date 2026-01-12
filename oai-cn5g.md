# OAI 5G Core Deployment Guide

Please refer to [OAI](https://gitlab.eurecom.fr/oai/openairinterface5g/-/blob/develop/doc/NR_SA_Tutorial_OAI_CN5G.md?ref_type=heads) for the official documentation.

## System Preparation

### Hardware Requirements

Ensure your machine meets the following specifications:

| Component   | Minimum Specification | Recommended Specification |
| :---------- | :-------------------- | ------------------------- |
| **OS**      | Ubuntu 22.04 LTS      | Ubuntu 22.04 LTS          |
| **CPU**     | 4 cores               | 8 cores x86_64 @ 3.5 GHz  |
| **RAM**     | 16 GB                 | 32 GB                     |
| **Network** | 1 Interface           | 2 Interfaces              |

### Install Pre-requisites

```bash
# 1. Update and install basic tools
sudo apt update
sudo apt install -y git net-tools putty unzip ca-certificates curl

# 2. Add Docker's official GPG key
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# 3. Add the repository to Apt sources
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# 4. Install Docker packages
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin docker-compose

# 5. Add current user to docker group (avoids using sudo for docker)
sudo usermod -a -G docker $(whoami)
```

> [!TIP]
> Log out and back in to apply group changes or run `su - $USER` to start a new session.

### Enable IP Forwarding

```bash
sudo sysctl net.ipv4.conf.all.forwarding=1
sudo iptables -P FORWARD ACCEPT
```

## 5G Core Deployment

**1. Download Configuration Files**

```bash
wget -O ~/oai-cn5g.zip https://gitlab.eurecom.fr/oai/openairinterface5g/-/archive/develop/openairinterface5g-develop.zip?path=doc/tutorial_resources/oai-cn5g
unzip ~/oai-cn5g.zip
mv ~/openairinterface5g-develop-doc-tutorial_resources-oai-cn5g/doc/tutorial_resources/oai-cn5g ~/oai-cn5g
rm -r ~/openairinterface5g-develop-doc-tutorial_resources-oai-cn5g ~/oai-cn5g.zip
```

**2. Pull Images & Start**

```bash
cd ~/oai-cn5g
docker compose pull
docker compose up -d
```

**Stop the Network**

```bash
cd ~/oai-cn5g
docker compose down
```

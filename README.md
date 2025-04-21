
---

# FRP Server Setup Guide

**by Dr3amforg3**

This guide will help you set up FRP (Fast Reverse Proxy) on both a Linux VPS (like Ubuntu) and Windows, creating a tunnel for your server's communication.

---

## **Linux Setup**

### 1. **Set Up FRP Server on Linux VPS (Ubuntu)**

Run the following commands one by one:

```sh
sudo apt update && sudo apt upgrade -y
```

```sh
wget https://github.com/fatedier/frp/releases/download/v0.62.0/frp_0.62.0_linux_amd64.tar.gz
```

```sh
tar -xzf frp_0.62.0_linux_amd64.tar.gz
```

```sh
cd frp_0.62.0_linux_amd64
```

```sh
nano frps.ini
```

In the `frps.ini` file, copy and paste the following configuration:

```
[common]
bind_port = 7000
dashboard_port = 7500
dashboard_user = admin
dashboard_pwd = yourpass
token = supersecret
```

(Press `Ctrl+X`, then `Y`, and hit `Enter` to save the file.)

---

### 2. **Create the Systemd Service for FRP**

```sh
sudo nano /etc/systemd/system/frps.service
```

In the `frps.service` file, copy and paste the following content:

```
[Unit]
Description=FRP Server Service
After=network.target

[Service]
User=root
WorkingDirectory=/root/frp_0.62.0_linux_amd64
ExecStart=/root/frp_0.62.0_linux_amd64/frps -c /root/frp_0.62.0_linux_amd64/frps.ini
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

(Press `Ctrl+X`, then `Y`, and hit `Enter` to save the file.)

---

### 3. **Reload Systemd and Enable the Service**

```sh
sudo systemctl daemon-reload
```

```sh
sudo systemctl enable frps
```

```sh
sudo systemctl start frps
```

---

### 4. **Open Necessary Ports on Your VPS**

You need to open the following ports in your VPS network settings:

- **7000**: FRP communication
- **3724**: Authentication TCP
- **8085**: World TCP

You may need to adjust your VPS firewall or use the provider's control panel to allow these ports.

---

## **Windows Setup**

### 1. **Download FRP for Windows**

Download the latest release for Windows from the link below. Note that it might show a virus protection warning due to its tunneling nature. Allow the download and proceed with the setup.

[Download FRP for Windows](https://github.com/fatedier/frp/releases/download/v0.62.0/frp_0.62.0_windows_amd64.zip)

---

### 2. **Create `frpc.ini` File**

In the same folder as `frpc.exe`, create a new file called `frpc.ini` and paste the following configuration:

```
[common]
server_addr = YOUR.VPS.IP
server_port = 7000
token = supersecret

[auth_tcp]
type = tcp
local_ip = 127.0.0.1
local_port = 3724
remote_port = 3724

[world_tcp]
type = tcp
local_ip = 127.0.0.1
local_port = 8085
remote_port = 8085
```

Replace `YOUR.VPS.IP` with your VPS's actual IP address.

---

### 3. **Configure Realmlist**

Finally, in your WoW server's database, set the `realmlist` IP address to your VPS's IP.

---

That's it! You should now have a fully working FRP setup on both Linux and Windows. Let me know if you need any further help!

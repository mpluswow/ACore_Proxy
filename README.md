by Dr3amforg3


1. On Linux VPS like Ubuntu copy and paste each command one by one:

Commands:
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

Copy and Paste this (This will make it runn 24/7) :

```
[common]
bind_port = 7000
dashboard_port = 7500
dashboard_user = admin
dashboard_pwd = yourpass
token = supersecret
```

---(press ctrl+x than Y than enter to save it.)

```sh
sudo nano /etc/systemd/system/frps.service
```

---Copy and Paste this:

```
[Unit]
Description=FRP Server Service
After=network.target

[Service]
Type=simple
User=root
WorkingDirectory=/home/root/frp_0.62.0_linux_amd64
ExecStart=/home/root/frp_0.62.0_linux_amd64/frps -c /home/your_username_here/frp_0.62.0_linux_amd64/frps.ini
Restart=on-failure

[Install]
WantedBy=multi-user.target
```


---(press ctrl+x than Y than enter to save it.)

```sh
sudo systemctl daemon-reload
```

```sh
sudo systemctl start frps
```

```sh
sudo systemctl enable frps
```


2. Open Ports on Your VPS probably network settings on the VPS website:

[tunel_port] 
type = tcp
7000

[auth_tcp]
type = tcp
3724

[world_tcp]
type = tcp
8085



WINDOWS PART:

This file can show virus protection alert so You need to allow it to download, 
it create tunel thats why Windows thinks its dangerous.
When you get a notyfication that threat was found, click on noptyfication , select allow by pressing arrow down next to file name.


```
https://github.com/fatedier/frp/releases/download/v0.62.0/frp_0.62.0_windows_amd64.zip
```

In the same folder as frpc.exe, create frpc.ini and paste this:

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

in your database set realmlist ip adress to your VPS ip.

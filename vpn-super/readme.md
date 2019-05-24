# How to setup vpn on ubuntu 18.04

```bash
apt-get update && apt-get install python-pip
pip install shadowsocks
apt-get install python-m2crypto
apt-get install build-essential
wget https://github.com/jedisct1/libsodium/releases/download/1.0.10/libsodium-1.0.10.tar.gz
tar xf libsodium-1.0.10.tar.gz && cd libsodium-1.0.10
./configure && make && make install
ldconfig
cd /etc && touch shadowsocks.json && nano shadowsocks.json
```

Write config and save

```json
{
	"server":"your_droplet's_IP_address",
	"server_port":8088,
	"local_port":1088,
	"password":"your_password",
	"timeout":600,
	"method":"aes-256-cfb"
}
```

upgrade to latest version and start the server

```bash
pip install -U git+https://github.com/shadowsocks/shadowsocks.git@master
ssserver -c /etc/shadowsocks.json -d start
```

auto run on startup

```bash
printf '%s\n' '#!/bin/bash' '/usr/bin/python /usr/local/bin/ssserver -c /etc/shadowsocks.json -d start' 'exit 0' | sudo tee -a /etc/rc.local
sudo chmod +x /etc/rc.local
reboot
```

slightly modified and brief version of this article https://mighil.com/how-to-setup-shadowsocks-server-on-digitalocean-vps/

## Mac client is within this directory
1. Install
2. Edit server preference to match the ip/server_port/password/algorithm above. Leave the plugin related things empty
3. Start stop shadowsox when needed

SSH to root@ip and install ee
```bash
wget -qO ee rt.cx/ee && sudo bash ee
```

Create site
```bash
ee site create test.com --mysql --php7
```

Get packages and setup things

```bash
apt-get install apt-transport-https lsb-release ca-certificates -y
wget -O /etc/apt/trusted.gpg.d/php.gpg https://packages.sury.org/php/apt.gpg
echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" > /etc/apt/sources.list.d/php.list
apt-get update
apt install php7.2-fpm php7.2-xml php7.2-bz2  php7.2-zip php7.2-mysql  php7.2-intl php7.2-gd php7.2-curl php7.2-soap php7.2-mbstring -y
wget -O /etc/php/7.2/fpm/pool.d/www.conf https://raw.githubusercontent.com/VirtuBox/ubuntu-nginx-web-server/master/etc/php/7.2/fpm/pool.d/www.conf
#service php7.2-fpm restart
wget -O /etc/nginx/conf.d/upstream.conf https://raw.githubusercontent.com/VirtuBox/ubuntu-nginx-web-server/master/etc/nginx/conf.d/upstream.conf
#service nginx reload
cd /etc/nginx/common
wget https://raw.githubusercontent.com/VirtuBox/ubuntu-nginx-web-server/master/common.zip
apt install unzip -y
unzip common.zip
cp -f /etc/php/7.0/fpm/pool.d/www.conf /etc/php/7.2/fpm/pool.d/www.conf
```

Edit port from 9070 to 9080
```bash
nano /etc/php/7.2/fpm/pool.d/www.conf
nano /etc/nginx/conf.d/upstream.conf
```

Restart
```bash
service php7.2-fpm restart
service nginx reload
```

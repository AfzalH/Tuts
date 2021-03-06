SSH to root@ip and install ee
```bash
wget -qO ee rt.cx/ee && sudo bash ee
```

Create site
```bash
ee site create test.com --mysql --php7
```
Refresh DNS on Mac (another terminal)
```bash
sudo killall -HUP mDNSResponder 
```

Get packages and setup things

```bash
apt-get install apt-transport-https lsb-release ca-certificates -y
wget -O /etc/apt/trusted.gpg.d/php.gpg https://packages.sury.org/php/apt.gpg
echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" > /etc/apt/sources.list.d/php.list
apt-get update
apt install php7.2-fpm php7.2-xml php7.2-bz2  php7.2-zip php7.2-mysql  php7.2-intl php7.2-gd php7.2-curl php7.2-soap php7.2-mbstring -y
```

```bash
cp -f /etc/php/7.0/fpm/pool.d/www.conf /etc/php/7.2/fpm/pool.d/www.conf
```

Edit port from 9070 to 9080
```bash
nano /etc/php/7.2/fpm/pool.d/www.conf
nano /etc/nginx/conf.d/upstream.conf
```
Configure PHP
```bash
sed -i 's/upload_max_filesize = 2M/upload_max_filesize = 20M/g' /etc/php/7.2/fpm/php.ini
sed -i 's/max_execution_time = 30/max_execution_time = 300/g' /etc/php/7.2/fpm/php.ini
sed -i 's/max_input_time = 60/max_input_time = 300/g' /etc/php/7.2/fpm/php.ini
sed -i 's/post_max_size = 8M/post_max_size = 28M/g' /etc/php/7.2/fpm/php.ini
sed -i 's/memory_limit = 128M/memory_limit = 512M/g' /etc/php/7.2/fpm/php.ini
#
```
Restart
```bash
service php7.2-fpm restart
service nginx reload
```

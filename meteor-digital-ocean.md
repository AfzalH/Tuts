# create a droplet on digitalocean

Create a droplet and login using ssh 
(assuming you already have developed the meteor app in your local machine)

# Update apt get

```
apt-get update
```
# Install nginx

```
apt-get install nginx
```

# move to nginx virtual host config dir and create a config file and open it (change todos to your desired name)

```
cd /etc/nginx/sites-available
touch todos.conf
nano todos.conf
```

# put the following content and save with ctrl+o
```
server_tokens off;
map $http_upgrade $connection_upgrade {
    default upgrade;
    ''      close;
}

server{
    listen 80;
    server_name todos.srizon.com;
    root html;
    index index.html;
    if ($http_user_agent ~ "MSIE" ) {
        return 303 https://browser-update.org/update.html;
    }
    location / {
        proxy_pass http://127.0.0.1:8080;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade; # allow websockets
        proxy_set_header Connection $connection_upgrade;
        proxy_set_header X-Forwarded-For $remote_addr; # preserve client IP

        # this setting allows the browser to cache the application in a way compatible with Meteor
        # on every applicaiton update the name of CSS and JS file is different, so they can be cache infinitely (here: 30 days)
        # the root path (/) MUST NOT be cached
        if ($uri != '/') {
            expires 30d;
        }
    }
}

```
# link it to sites-enabled
```
ln -s /etc/nginx/sites-available/todos.conf /etc/nginx/sites-enabled/todos.conf
service nginx restart
```

# setup mongo
```
apt-get install mongodb-server
```

# install node
```
apt-get install npm
npm install --global --save n
n 4.4.5
```

# add a user for this app (change todos)
```
adduser --disabled-login todos
```

# create starupt script
```
touch /etc/init/todos.conf
nano /etc/init/todos.conf
```

# enter the content and save with ctrl+o and ctrl+x
```
# upstart service file at /etc/init/todos.conf
description "Meteor.js (NodeJS) application"
author "Daniel Speichert <daniel@speichert.pro>"

# When to start the service
start on started mongodb and runlevel [2345]

# When to stop the service
stop on shutdown

# Automatically restart process if crashed
respawn
respawn limit 10 5

# we don't use buil-in log because we use a script below
# console log

# drop root proviliges and switch to mymetorapp user
setuid todos
setgid todos

script
    export PATH=/opt/local/bin:/opt/local/sbin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
    export NODE_PATH=/usr/lib/nodejs:/usr/lib/node_modules:/usr/share/javascript
    # set to home directory of the user Meteor will be running as
    export PWD=/home/todos
    export HOME=/home/todos
    # leave as 127.0.0.1 for security
    export BIND_IP=127.0.0.1
    # the port nginx is proxying requests to
    export PORT=8080
    # this allows Meteor to figure out correct IP address of visitors
    export HTTP_FORWARDED_COUNT=1
    # MongoDB connection string using todos as database name
    export MONGO_URL=mongodb://localhost:27017/todos
    # The domain name as configured previously as server_name in nginx
    export ROOT_URL=http://todos.srizon.com
    # optional JSON config - the contents of file specified by passing "--settings" parameter to meteor command in development mode
    # export METEOR_SETTINGS='{ "somesetting": "someval", "public": { "othersetting": "anothervalue" } }'
    # this is optional: http://docs.meteor.com/#email
    # commented out will default to no email being sent
    # you must register with MailGun to have a username and password there
    # export MAIL_URL=smtp://postmaster@mymetorapp.net:password123@smtp.mailgun.org
    # alternatively install "apt-get install default-mta" and uncomment:
    # export MAIL_URL=smtp://localhost
    exec node /home/todos/bundle/main.js >> /home/todos/todos.log
end script
root@todos:/etc/nginx/sites-available# 

```
# build the app locally (different terminal - localhost)
```
cd /app/dir
meteor build .
scp scp filename.tar.gz root@todos.net:/root/
```
# on server again move and extract the file and npm install
```
mkdir /home/todos
mv filename.tar.gz /home/todos
cd /home/todos
tar -zxf filename.tar.gz
cd /home/todos/bundle/programs/server
npm install
chown todos:todos /home/todos -R
start todos
```

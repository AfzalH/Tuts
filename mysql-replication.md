# Mysql Server Replication

## Master Server Setup (ubuntu - assuming mysql is already installed)

### Command (terminal one)

	ssh root@maseter.server.ip.address
	nano /etc/mysql/my.cnf

### Edit these lines

	bind-address            = 127.0.0.1
into

	bind-address            = master.server.ip.address
then

	#server-id               = 1
into

	server-id               = 1
then

	#log_bin                 = /var/log/mysql/mysql-bin.log
into

	log_bin                 = /var/log/mysql/mysql-bin.log
then

	#binlog_do_db            = newdatabase
into

	binlog_do_db            = your_database_name

### command (terminal one)

	service mysql restart
	mysql -u root -p
	create user 'slave'@'%' identified by 'slavepassword';
	grant replication slave on *.* to 'slave'@'%'; 
	flush privileges;
	use your_database_name;
	FLUSH TABLES WITH READ LOCK;
	SHOW MASTER STATUS;

### Note
note the values of File and Position after the command `show master status` it will be needed for slave setup.

keep terminal one open and open a new terminal and ssh to server

### command (terminal two)

	mysqldump -u root -p your_database_name > your_database_name.sql
	scp your_database_name.sql root@slave.server.ip.address:\root\
	exit

### command (terminal one)

	unlock tables;
	exit;
	exit;

## Slave Server Setup (ubuntu)

### Command (terminal one)

	ssh root@slave.server.ip.address
	apt-get update
	apt-get install mysql-server mysql-client
	mysql -u root -p
	create database your_database_name;
	exit;
	mysql -u root -p your_database_name < your_database_name.sql
	nano /etc/mysql/my.cnf

### Edit the lines

	bind-address            = 127.0.0.1
into

	bind-address            = slave.server.ip.address
then

	#server-id               = 1
into

	server-id               = 2
then

	#log_bin                 = /var/log/mysql/mysql-bin.log
into

	log_bin                 = /var/log/mysql/mysql-bin.log
then

	#relay-log               = /var/log/mysql/mysql-relay-bin.log
into

	relay-log               = /var/log/mysql/mysql-relay-bin.log
then

	#binlog_do_db            = newdatabase
into

	binlog_do_db            = your_database_name

### Command (terminal one)
	
	service mysql restart
	mysql -u root -p
	CHANGE MASTER TO MASTER_HOST='master.server.ip.address',MASTER_USER='slave', MASTER_PASSWORD='slavepassword', MASTER_LOG_FILE='mysql-bin.000001', MASTER_LOG_POS=  107;

*Note:* enter the logfile name and pos value according to the values noted before after running show master status command

	start slave;
	show slave status\G;

Done!

```php
<?php
echo "Hello World!";
for($i=0;$i<10;$i++){
	echo $i;
}
```



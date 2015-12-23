# Installing JasperServer on Centos (tested version 6.7)

## Man! it was hard to accomplish
### command

	yum install wget
	yum install unzip
	yum install nano
	yum install java-1.7.0-openjdk
	yum install java-1.7.0-openjdk-devel
	yum install postgresql-server
	yum install postgresql-contrib
	wget http://www.us.apache.org/dist/tomcat/tomcat-7/v7.0.67/bin/apache-tomcat-7.0.67.tar.gz
	tar xzf apache-tomcat-7.0.67.tar.gz
	rm -f apache-tomcat-7.0.67.tar.gz
	mv apache-tomcat-7.0.67 /usr/local/tomcat7
	/usr/local/tomcat7/bin/startup.sh
	wget http://skylineservers.dl.sourceforge.net/project/jasperserver/JasperServer/JasperReports%20Server%20Community%20Edition%206.2.0/jasperreports-server-cp-6.2.0-bin.zip
	unzip jasperreports-server-cp-6.2.0-bin.zip
	rm -f jasperreports-server-cp-6.2.0-bin.zip
	cd jasperreports-server-cp-6.2.0-bin/buildomatic
	cp sample_conf/postgresql_master.properties ./
	mv postgresql_master.properties default_master.properties
	nano default_master.properties

find and edit the following
appServerType = tomcat7
appServerDir = /usr/local/tomcat7

	service postgresql initdb
	service postgresql start
	chkconfig postgresql on
	nano /var/lib/pgsql/data/pg_hba.conf

change ident to trust

	service postgresql restart

change hostname to localhost

	hostname localhost
	./js-install-ce.sh
	cd /usr/local/tomcat7/bin
	./shutdown.sh
	./startup.sh

Now http://ip.add.re.ss:8080/jasperserver should load it

os ubuntu 16.04.1 ... $5 droplet on DigitalOcean
ssh to droplet

	wget -qO ee rt.cx/ee && sudo bash ee

you will be asked to provide your name and email

	ee stack install --nginx
	wget https://raw.githubusercontent.com/frappe/bench/master/playbooks/install.py
	python install.py --develop --user frappe

you will be asked to provide mysql root password and admin password

	su - frappe
	cd frappe-bench
	bench new-site f.srizon.com

you will be asked to provide mysql root password and set new admin password

	bench start

the site should be accessible at `http://f.srizon.com:8000/` use username administrator and the password you just set.
you may also need refreshing if the layout seems broken
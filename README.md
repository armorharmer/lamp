# LAMP Stack + WordPress instalacija i podešavanje

## Update operativnog sistema

	yum update

## Instaliranje paketa

	yum install wget nano

## Disable firewall

	systemctl disable firewalld

## Disable selinux and reboot

	nano /etc/selinux/config
	
I namestiti opciju SELINUX na permissive:

	SELINUX=permissive

Da bi se sačuvao fajl u Nano editoru, potrebno je pritisnuti CTRL+X, zatim dugme y i na kraju Enter. Posle toga bi trebalo da se vratite u Linux prompt.

Zatim uraditi reboot komandom reboot.

## Instalacija remi i epel repozitorijuma

	wget https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
	wget http://rpms.remirepo.net/enterprise/remi-release-7.rpm

	rpm -Uvh epel-release-latest-7.noarch.rpm
	rpm -Uvh remi-release-7.rpm


## Osveziti repozitorijume

	yum update

## Instaliranje Apache HTTP web servera

	yum install httpd
	systemctl enable httpd
	systemctl start httpd

## Instaliranje MySQL (MariaDB) baze podataka

	yum install mariadb-server
	systemctl start mariadb
	systemctl enable mariadb

## Podesavanje baze podataka

	mysql_secure_installation

	Enter current password for root (enter for none): ENTER
	Set root password? [Y/n] y
	New password:  UKUCAJTE SIFRU
	Re-enter new password: PONOVO UKUCAJTE SIFRU
	Remove anonymous users? [Y/n] y
	Disallow root login remotely? [Y/n] y
	Remove test database and access to it? [Y/n] y
	Reload privilege tables now? [Y/n] y

## PHP instalacija (7.0) - podesavanje REMI repozitorijuma

	nano /etc/yum.repos.d/remi-php70.repo

Naći deo sa [remi-php70] i ukljuciti ga:

> enabled=1

## PHP instalacija (7.0) - update package db

	yum update

## PHP instalacija (7.0) - instaliranje paketa
 
	yum install php php-mysql php-gd php-pear 

## PHP instalacija (7.0) - proveravanje php verzije

	php -v

## Download WordPress

	cd /root
	wget https://wordpress.org/latest.tar.gz 
	tar xvzf latest.tar.gz
	cd wordpress
	mv * /var/www/html

## Podešavanje privilegija

	chown apache:apache /var/www/html -R

## Restart httpd servisa

	systemctl restart httpd

## Instalacija WordPress - podešavanje baze - kreiranje baze podataka i korisnika:

Ulogovati se u bazu komandom:

	mysql -u root -p

Zatim ukucati lozinku i u MySQL promptu ukucati:

	CREATE DATABASE wpdb;
	CREATE USER 'wpkorisnik'@'localhost' IDENTIFIED BY 'lozinka123';
	GRANT ALL PRIVILEGES ON wpdb.* TO 'wpkorisnik'@'localhost';
	FLUSH PRIVILEGES;

Izaći komandom:

	quit

## Instalacija WordPress

Saznati ip adresu virtalne mašine komandom:

	ip addr show

Ukucati ip adresu virtualne mašine u URL bar u web browser.

I instalirati WordPress unošenjem podataka.

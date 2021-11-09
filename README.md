# Jarkom-Modul-3-IUP1-2021

## Group Member Name:
1. (05111942000010) Agustinus Aldi Irawan
2. (05111942000017) Juan Carlos Tepanus Pardosi
3. (05111942000028) Salma Izzatul Islam

#### Foosha (Router)
``` 
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
	address 10.38.1.1
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 10.38.2.1
	netmask 255.255.255.0 
	
auto eth3
iface eth3 inet static
	address 10.38.3.1
	netmask 255.255.255.0
 ```
 
 #### Loguetown (Client)
```
#auto eth0
#iface eth0 inet static
#	address 10.38.1.2
#	netmask 255.255.255.0
#	gateway 10.38.1.1

auto eth0
iface eth0 inet dhcp
```

#### Alabasta (Client)
```
#auto eth0
#iface eth0 inet static
#	address 10.38.1.3
#	netmask 255.255.255.0
#	gateway 10.38.1.1

auto eth0
iface eth0 inet dhcp

```

#### EniesLobby (DNS Server)
```
auto eth0
iface eth0 inet static
	address 10.38.2.2
	netmask 255.255.255.0
	gateway 10.38.2.1

```


#### Water7 (Proxy Server)
```
auto eth0
iface eth0 inet static
	address 10.38.2.3
	netmask 255.255.255.0
	gateway 10.38.2.1

```

#### Jipangu (DHCP Server)

```
auto eth0
iface eth0 inet static
	address 10.38.2.4
	netmask 255.255.255.0
	gateway 10.38.2.1
```

#### TottoLand (Client)

```
auto eth0
iface eth0 inet dhcp
```

#### Skypie (Client)

```
#auto eth0
#iface eth0 inet static
#	address 10.38.3.3
#	netmask 255.255.255.0
#	gateway 10.38.3.1

auto eth0
iface eth0 inet dhcp
hwaddress ether 66:f9:90:6c:ae:ae
```

## All

### ./.bashrc
```
/root/config.sh
```

### Terminal
```
chmod +x config.sh

Foosha
service isc-dhcp-relay stop
service isc-dhcp-relay start

EniesLobby
service bind9 restart
service bind9 stop

Jipangu
service isc-dhcp-server start
service isc-dhcp-server restart
service isc-dhcp-server status
dhcpd --version

Water 7
service squid restart
service squid status
touch /etc/squid/passwd

htpasswd -cm /etc/squid/passwd luffybelikapalIUP1
luffy_IUP1
luffy_IUP1

htpasswd -m /etc/squid/passwd zorobelikapalIUP1
zoro_IUP1
zoro_IUP1

Client
export http_proxy=http://10.38.2.3:5000
env | grep -i proxy
unset http_proxy

ps aux | grep ping
kill -9 [pid]
```

### resolv.conf
```
nameserver 192.168.122.1
nameserver 10.38.2.2
nameserver 10.38.2.3
nameserver 10.38.2.4
```

## Foosha (Router)
### ./.bashrc
```
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 10.38.0.0/16
/root/config.sh
```

### config.sh
```
#!/bin/sh

apt-get update
apt-get install isc-dhcp-relay -y
#iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 10.38.0.0/16

cp /root/isc-dhcp-relay /etc/default
cp /root/sysctl.conf /etc

service isc-dhcp-relay start
```

### /etc/defaultisc-dhcp-relay
```
# What servers should the DHCP relay forward requests to?
SERVERS="10.38.2.4"

# On what interfaces should the DHCP relay (dhrelay) serve DHCP requests?
INTERFACES="eth1 eth2 eth3"

# Additional options that are passed to the DHCP relay daemon?
OPTIONS=""
```

## EniesLobby (DNS Server)

### config.sh
```
#!/bin/sh

echo nameserver 192.168.122.1 > /etc/resolv.conf

apt-get update
apt-get install bind9 -y

service bind9 start

cp /root/named.conf.options /etc/bind
cp /root/named.conf.local /etc/bind

mkdir /etc/bind/kaizoku

cp /root/proxy/super.franky.IUP1.com /etc/bind/kaizoku
cp /root/franky.IUP1.com /etc/bind/kaizoku
cp /root/2.38.10.in-addr.arpa /etc/bind/kaizoku
```

### named.conf.options
```
        //forwarders {
        //      192.168.122.1
        //};

        //=====================================================================$
        // If BIND logs error messages about the root key being expired,
        // you will need to update your keys.  See https://www.isc.org/bind-keys
        //=====================================================================$
        //dnssec-validation auto;
        allow-query { any; };

        auth-nxdomain no;    # conform to RFC1035
        listen-on-v6 { any; };
};
```

### named.conf.options
```
//
// Do any local configuration here
//

// Consider adding the 1918 zones here, if they are not used in your
// organization
//include "/etc/bind/zones.rfc1918";

zone "franky.IUP1.com" {
        type master;
        notify yes;
        also-notify { 10.38.2.3; };
        allow-transfer { 10.38.2.3; };
        file "/etc/bind/kaizoku/franky.IUP1.com";
};

zone "super.franky.IUP1.com" {
        type master;
        file "/etc/bind/kaizoku/super.franky.IUP1.com";
        allow-transfer { 10.38.3.69; };
};

zone "2.38.10.in-addr.arpa" {
        type master;
        file "/etc/bind/kaizoku/2.38.10.in-addr.arpa";
};
```

## Water7 (Proxy Server)

### config.sh
```

#!/bin/sh

echo nameserver 192.168.122.1 > /etc/resolv.conf

apt-get update
apt-get install squid -y
apt-get install apache2-utils -y

cp /root/resolv.conf /etc
cp /root/acl.conf /etc/squid
cp /root/squid.conf /etc/squid
cp /root/passwd /etc/squid
cp /root/acl-bandwidth.conf /etc/squid

cp /root/named.conf.local /etc/bind
cp /root/named.conf.options /etc/bind

mkdir /etc/bind/sunnygo

cp /root/mecha.franky.IUP1.com /etc/bind/sunnygo
cp /root/general.mecha.franky.IUP1.com /etc/bind/sunnygo

service squid start
service squid status
```

### acl.conf
```
acl AVAILABLE_WORKING_1 time S 00:00-23:59
acl AVAILABLE_WORKING_2 time MT 00:00-06:59
acl AVAILABLE_WORKING_3 time M 11:01-23:59
acl AVAILABLE_WORKING_4 time TWH 11:01-16:59
acl AVAILABLE_WORKING_5 time WH 03:01-06:59
acl AVAILABLE_WORKING_6 time F 03:01-16:59
acl AVAILABLE_WORKING_7 time A 03:01-23:59
```

### passwd
```
luffybelikapalIUP1:$apr1$ZDmOEh1W$klxkxjHGrKJbZHdqxQecd/
zorobelikapalIUP1:$apr1$f6R5iv09$CzVv1d5k.rI1bQSG3q0KN.
```

### squid.conf
```
include /etc/squid/acl.conf
include /etc/squid/acl-bandwidth.conf

http_port 5000
visible_hostname jualbelikapal.IUP1.com

http_access deny AVAILABLE_WORKING_1
http_access deny AVAILABLE_WORKING_2
http_access deny AVAILABLE_WORKING_3
http_access deny AVAILABLE_WORKING_4
http_access deny AVAILABLE_WORKING_5
http_access deny AVAILABLE_WORKING_6
http_access deny AVAILABLE_WORKING_7

auth_param basic program /usr/lib/squid/basic_ncsa_auth /etc/squid/passwd
auth_param basic children 5
auth_param basic realm Proxy
auth_param basic credentialsttl 2 hours
auth_param basic casesensitive on
acl USERS proxy_auth REQUIRED
http_access allow USERS

acl BLACKLIST dstdomain google.com
deny_info http://super.franky.IUP1.com/ BLACKLIST
#http_reply_access deny BLACKLIST
```

### acl-bandwidth.conf
```
acl download url_regex -i \.jpg$ \.png$

auth_param basic program /usr/lib/squid/basic_ncsa_auth /etc/squid/passwd

acl luffy proxy_auth luffybelikapalIUP1
acl zoro proxy_auth zorobelikapalIUP1

delay_pools 2
delay_class 1 1
delay_parameters 1 1250/1250
delay_access 1 allow luffy
delay_access 1 deny zoro
delay_access 1 allow download
delay_access 1 deny all

delay_class 2 1
delay_parameters 2 -1/-1
delay_access 2 allow zoro
delay_access 2 deny luffy
delay_access 2 deny all
```

## Jipangu (DHCP Server)

### config.sh
```
#!/bin/sh

echo nameserver 192.168.122.1 > /etc/resolv.conf
apt-get update
apt-get install isc-dhcp-server -y

dhcpd --version

cp /root/isc-dhcp-server /etc/default
cp /root/dhcpd.conf /etc/dhcp

service isc-dhcp-server start
service isc-dhcp-server restart
service isc-dhcp-server status
```

### dhcpd.conf
```
subnet 10.38.1.0 netmask 255.255.255.0 {
    range 10.38.1.20 10.38.1.99;
    range 10.38.1.150 10.38.1.169;
    option routers 10.38.1.1;
    option broadcast-address 10.38.1.255;
    option domain-name-servers 10.38.2.2;
    default-lease-time 360;
    max-lease-time 7200;
}

subnet 10.38.3.0 netmask 255.255.255.0 {
    range 10.38.3.30 10.38.3.50;
    option routers 10.38.3.1;
    option broadcast-address 10.38.3.255;
    option domain-name-servers 10.38.2.2;
    default-lease-time 720;
    max-lease-time 7200;
}

subnet 10.38.2.0 netmask 255.255.255.0 {
        option routers 10.38.2.1;
}

host Skypie {
    hardware ethernet 66:f9:90:6c:ae:ae;
    fixed-address 10.38.3.69;
}
```

### isc-dhcp-server
```
INTERFACES="eth0"
```

## Alabasta
### config.sh
```
#!/bin/sh

echo nameserver 192.168.122.1 > /etc/resolv.conf
apt-get update
apt-get install lynx -y
apt-get install speedtest-cli -y

#echo nameserver 10.38.2.2 > /etc/resolv.conf
cp /root/resolv.conf /etc

cp intconf.txt /etc/network/interfaces

#export http_proxy=http://10.38.2.3:5000
```

### interfaces
```
#auto eth0
#iface eth0 inet static
#       address 10.38.1.2
#       netmask 255.255.255.0
#       gateway 10.38.1.1

auto eth0
iface eth0 inet dhcp
```

## LogueTown
### config.sh
```
#!/bin/sh

echo nameserver 192.168.122.1 > /etc/resolv.conf
apt-get update
apt-get install lynx -y
apt-get install speedtest-cli -y

#echo nameserver 10.38.2.2 > /etc/resolv.conf
cp /root/resolv.conf /etc

cp intconf.txt /etc/network/interfaces

#export http_proxy=http://10.38.2.3:5000
```

## Skypie
### config.sh
```
#!/bin/sh

echo nameserver 192.168.122.1 > /etc/resolv.conf
apt-get update
apt-get install apache2 -y
apt-get install php -y
apt-get install libapache2-mod-php7.0 -y
apt-get install wget -y
apt-get install unzip -y
apt-get install lynx -y

#echo nameserver 10.38.2.2 > /etc/resolv.conf
#echo nameserver 10.38.3.3

cp /root/resolv.conf /etc
cp franky.IUP1.com.conf /etc/apache2/sites-available
cp /root/apache2.conf /etc/apache2

mkdir /var/www/franky.IUP1.com

cp /root/franky/home.html /var/www/franky.IUP1.com
cp /root/franky/index.php /var/www/franky.IUP1.com

a2ensite franky.IUP1.com
service apache2 restart

a2enmod rewrite
service apache2 restart

mkdir /var/www/super.franky.IUP1.com

cp /root/.htaccess /var/www/franky.IUP1.com
cp super.franky.IUP1.com.conf /etc/apache2/sites-available
cp super.franky.IUP1.com /var/www

mkdir /var/www/super.franky.IUP1.com/error

mkdir /var/www/super.franky.IUP1.com/public
mkdir /var/www/super.franky.IUP1.com/public/css
mkdir /var/www/super.franky.IUP1.com/public/images
mkdir /var/www/super.franky.IUP1.com/public/js

cp /root/super.franky/error/404.html /var/www/super.franky.IUP1.com/error

cp /root/super.franky/public/css/bro.css /var/www/super.franky.IUP1.com/public/css
cp /root/super.franky/public/css/edit.css /var/www/super.franky.IUP1.com/public/css
cp /root/super.franky/public/css/main.css /var/www/super.franky.IUP1.com/public/css

cp /root/super.franky/public/images/background-frank.jpg /var/www/super.franky.IUP1.com/public/images
cp /root/super.franky/public/images/bukanfrankytapirandom.99689 /var/www/super.franky.IUP1.com/public/images
cp /root/super.franky/public/images/car.jpg /var/www/super.franky.IUP1.com/public/images
cp /root/super.franky/public/images/eyeoffranky.jpg /var/www/super.franky.IUP1.com/public/images
cp /root/super.franky/public/images/fake-franky.jpg234 /var/www/super.franky.IUP1.com/public/images
cp /root/super.franky/public/images/franky.png /var/www/super.franky.IUP1.com/public/images
cp /root/super.franky/public/images/frankysupersecretfood.cursed /var/www/super.franky.IUP1.com/public/images
cp /root/super.franky/public/images/not-franky.jpg /var/www/super.franky.IUP1.com/public/images
cp /root/super.franky/public/images/papa.franky.jpg /var/www/super.franky.IUP1.com/public/images
cp /root/super.franky/public/images/roboto-frankyasdjklkljqwennmn.listingbro /var/www/super.franky.IUP1.com/public/images
cp /root/super.franky/public/images/stockphotorandomfran.woi /var/www/super.franky.IUP1.com/public/images
cp /root/super.franky/public/images/whatMatters.jpg /var/www/super.franky.IUP1.com/public/images

cp /root/super.franky/public/js/autocomplete.js /var/www/super.franky.IUP1.com/public/js
cp /root/super.franky/public/js/js.js /var/www/super.franky.IUP1.com/public/js
cp /root/super.franky/public/js/reddit.js /var/www/super.franky.IUP1.com/public/js

a2ensite super.franky.IUP1.com
service apache2 restart

cp general.mecha.franky.IUP1.com.conf /etc/apache2/sites-available
cp ports.conf /etc/apache2

mkdir /var/www/general.mecha.franky.IUP1.com

cp /root/general.mecha.franky/drag.wav /var/www/general.mecha.franky.IUP1.com
cp /root/general.mecha.franky/duck-duck-go.jpg /var/www/general.mecha.franky.IUP1.com
cp /root/general.mecha.franky/f1.png /var/www/general.mecha.franky.IUP1.com
cp /root/general.mecha.franky/funko-pop.jpg /var/www/general.mecha.franky.IUP1.com

a2ensite general.mecha.franky.IUP1.com
service apache2 restart

cp 000-default.conf /etc/apache2/sites-available
```

### apache2.conf
```
ServerName 10.38.3.69
```

## TottoLand
### config.sh
```
#!/bin/sh

echo nameserver 192.168.122.1 > /etc/resolv.conf
apt-get update
apt-get install lynx -y
apt-get install speedtest-cli -y

#echo nameserver 10.38.2.2 > /etc/resolv.conf
cp /root/resolv.conf /etc

cp intconf.txt /etc/network/interfaces

#export http_proxy=http://10.38.2.3:5000
```

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
auto eth0
iface eth0 inet dhcp
hwaddress ether 4a:ed:9a:f1:bc:97
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
```

## Foosha (Router)
### ./.bashrc
```
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 10.38.0.0/16
/root/config.sh
```

### config.sh
```
# !/bin/sh

apt-get update
apt-get install isc-dhcp-relay -y
apt-get install nano

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
# !/bin/sh

echo nameserver 192.168.122.1 > /etc/resolv.conf

apt-get update
apt-get install bind9 -y

cp /root/named.conf.options /etc/bind/

service bind9 restart
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

## Water7 (Proxy Server)

### config.sh
```
#!/bin/sh

echo nameserver 192.168.122.1 > /etc/resolv.conf

apt-get update
apt-get install squid -y
apt-get install apache2-utils -y

cp /root/acl.conf /etc/squid
cp /root/squid.conf /etc/squid
cp /root/passwd /etc/squid

#cp /root/squid.conf /etc/squid
#cp /root/acl.conf /etc/squid
#cp /root/acl-bandwidth.conf /etc/squid
#cp /root/restrict-sites.acl /etc/squid
#htpasswd -c /etc/squid/passwd jarkom203

service squid restart
service squid status
```

### acl.conf
```
acl AVAILABLE_WORKING time MTWH 07:00-11:00
acl AVAILABLE_WORKING time TWHF 17:00-23:59
acl AVAILABLE_WORKING time WHFA 00:00-03:00
```

### passwd
```
luffybelikapaliup3:$apr1$.ngvk2dU$dI7CCjWGlLXCRAhPXc4YQ0
zorobelikapaliup3:$apr1$iXli46Lw$oid7trkNDuY6.BY2Dqz3u1
```

### squid.conf
```
include /etc/squid/acl.conf

http_port 5000
visible_hostname jualbelikapal.IUP1.com
auth_param basic program /usr/lib/squid/basic_ncsa_auth /etc/squid/passwd
auth_param basic children 5
auth_param basic realm Login
auth_param basic credentialsttl 2 hours
auth_param basic casesensitive on
acl USERS proxy_auth REQUIRED

http_access allow USERS AVAILABLE_WORKING
http_access deny all
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
    hardware ethernet 32:da:e0:de:26:d9;
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

echo nameserver 10.38.2.2 > /etc/resolv.conf

cp intconf.txt /etc/network/interfaces

#echo nameserver 192.168.122.1 > /etc/resolv.conf

#apt-get update
#apt-get install lynx -y
#apt install speedtest-cli

#cp /root/interfaces /etc/network
#cp /root/resolv.conf /etc

#htpasswd -c /etc/squid/passwd jarkom203

#export http_proxy="http://10.38.2.3:8080"
#env | grep -i proxy

#export PYTHONHTTPSVERIFY=0
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

echo nameserver 10.38.2.2 > /etc/resolv.conf

cp intconf.txt /etc/network/interfaces
```

## Skypie
### config.sh
```
#!/bin/sh

echo nameserver 192.168.122.1 > /etc/resolv.conf
apt-get update
apt-get install lynx -y

echo nameserver 10.38.2.2 > /etc/resolv.conf

cp intconf.txt /etc/network/interfaces
```

## TottoLand
### config.sh
```
#!/bin/sh

echo nameserver 192.168.122.1 > /etc/resolv.conf
apt-get update
apt-get install lynx -y

echo nameserver 10.38.2.2 > /etc/resolv.conf

cp intconf.txt /etc/network/interfaces
```

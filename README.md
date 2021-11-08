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
auto eth0
iface eth0 inet static
	address 10.38.1.2
	netmask 255.255.255.0
	gateway 10.38.1.1
```

#### Alabasta (Client)
```
auto eth0
iface eth0 inet static
	address 10.38.1.3
	netmask 255.255.255.0
	gateway 10.38.1.1

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
iface eth0 inet static
	address 10.38.3.2
	netmask 255.255.255.0
	gateway 10.38.3.1
```

#### Skypie (Client)

```
auto eth0
iface eth0 inet static
	address 10.38.3.3
	netmask 255.255.255.0
	gateway 10.38.3.1
```

## All

### ./.bashrc
```
/root/config.sh
```

### Terminal
```
chmod +x config.sh

EniesLobby
service bind9 restart
service bind9 stop

Jipangu
service isc-dhcp-server restart
service isc-dhcp-server status
dhcpd --version

Water 7
service squid restart
service squid status

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
```

## EniesLobby (DNS Server)

### config.sh
```
# !/bin/sh

echo nameserver 192.168.122.1 > /etc/resolv.conf

apt-get update
apt-get install bind9 -y
```

## Water7 (Proxy Server)

### config.sh
```
#!/bin/sh

echo nameserver 192.168.122.1 > /etc/resolv.conf

apt-get update
apt-get install squid -y
apt-get install apache2-utils -y
apt install speedtest-cli -y

cp /root/squid.conf /etc/squid
cp /root/acl.conf /etc/squid
cp /root/acl-bandwidth.conf /etc/squid
cp /root/restrict-sites.acl /etc/squid


#htpasswd -c /etc/squid/passwd jarkom203

service squid restart
service squid status
```

##Jipangu (DHCP Server)

### config.sh
```
#!/bin/sh

echo nameserver 192.168.122.1 > /etc/resolv.conf
apt-get update
apt-get install isc-dhcp-server -y

dhcpd --version

cp /root/isc-dhcp-server /etc/default
cp /root/dhcpd.conf /etc/dhcp

service isc-dhcp-server restart
service isc-dhcp-server status
```

### dhcpd.conf
```
subnet 10.38.0.0 netmask 255.255.255.0 {
    range 10.38.0.100 10.38.0.169;
    option routers 10.38.0.1;
    option broadcast-address 10.38.0.255;
    option domain-name-servers 10.38.2.2;
    default-lease-time 60;
    max-lease-time 60;
}

#host Jipangu {
#    hardware ethernet 92:33:3a:7b:c1:95;
#    fixed-address 10.38.0.150;
#}
```

### dhcpd.conf
```
INTERFACES="eth1"
```

## Alabasta
### config.sh
```
#!/bin/sh

#echo nameserver 192.168.122.1 > /etc/resolv.conf

#apt-get update
#apt-get install lynx -y
#apt install speedtest-cli

cp /root/interfaces /etc/network
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

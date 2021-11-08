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

EniesLobby & Water7
service bind9 restart
service bind9 stop

Skypie
service apache2 restart

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

## EniesLobby

### config.sh
```
# !/bin/sh

echo nameserver 192.168.122.1 > /etc/resolv.conf

apt-get update
apt-get install bind9 -y
```

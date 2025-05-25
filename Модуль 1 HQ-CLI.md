# Задание 1. Произведите базовую настройку устройств

* Меняем имя
  
Hostnamectl set-hostname hq-cli.au-team.irpo; exec bash

* производим базовую настройку устройств
  
Ip a

Mcedit /etc/net/ifaces/ens33/options

" DISABLED=no 

NM_CONTROLLED=no 

BOOTPROTO=static 

CONFIG_IPv4=yes"

* echo bash 192.168.200.14/28 > /etc/net/ifaces/ens33/ipv4address
  
Cat /etc/net/ifaces/ens33/ipv4address

* echo default via 192.168.200.1 > /etc/net/ifaces/ens33/ipv4route
  
Systemctl restart network

# Задание 11. Часовой пояс

Timedatectl set-timezone Europe/Moscow

Timedatectl status

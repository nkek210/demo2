# Задание 1. Произведите базовую настройку устройств

* 1.1 имя экороутера

En

Conf t

Hostname br-rtr.au-team.irpo

* 1.2 настройка IP-адресации

Do sh port br

te0 - ISP-BR

te1 – BR-Net (BR-SRV)

* создаем два интерфейса
  
Int isp

Ip address 172.16.5.14/28

Ex

Int lan 

Ip address 192.168.2.1/27

Ex

* связываем интерфейс с портами
  
Port te0

Service-instance isp

Encapsulation untagged

Connect ip interface isp

Ex

Port te1

Service-instance  lan

Encapsulation untagged

Connect ip interface lan

Ex

* настроим маршрут по умолчанию
  
Ip route 0.0.0.0/0 172.16.5.1

* временный DNS
  
Ip name-server 77.88.8.8

* настроим пул адресов для NAT
  
Ip nat pool INTERNET 192.168.2.1-192.168.2.30

Ip nat source dynamic inside-to-outside pool INTERNET overload 172.16.5.14

* пропишем внутренние и внешние интерфейсы
  
Int isp

Ip nat outside

Ex

Int lan

Ip nat inside

Ex

# Задание 3.2. Создание пользователя net_admin на маршрутизаторах HQ-RTR и BR-RTR

* Создание пользователя net_admin
  
Conf t

username net_admin 

password P@ssw0rd  

role admin

write

# Задание 6. Между офисами HQ и BR необходимо сконфигурировать IP-туннель

Создаем туннель на BR-RTR

* создаем интерфейс туннеля
  
Int tunnel.1

Ip address 10.10.10.10/30

Ip runnel 172.16.5.14 172.16.4.14 mode gre

ex

Write

* проверяем туннель из BR-RTR
  
Do ping 10.10.10.9

* настроим ospf по туннелю
  
Router ospf1 1

Network 10.10.10.8/30 area 0

Network 192.168.2.0/27 area 0

Area 0 authentication message-digest

Passive-interface isp

Passive-interface lan

# Задание 7. Настройка OSPF на BR-RTR

router ospf 1

router-id 100.10.10.2
  
Network 192.168.4.1 0.0.0.0 area 1

Ex

Tunnel.1

Ip address 10.10.10.10/30

Ip tunnel 172.16.5.2 172.16.4.2

ex

# Задание 8. Настройка динамической трансляции адресов

int int1

  ip nat inside
  
!

int int0

  ip nat outside
  
Вся настройка выше - задание 1.2

* Создаем туннель: задание 2
  
Проверка туннель 

Ping 10.10.10.9

# Задание 11. Часовой пояс

show ntp ?

Show ntp date

Ntp timezone utc+3

show ntp timezone

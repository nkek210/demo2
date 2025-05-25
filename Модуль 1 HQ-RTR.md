# Задание 1.1 Настройка полного имени устройства

Hostname hq-rtr.au-team.irpo

# Задание 1.2 Настройка IP-адресации на эко-роутере 

En

Conf t

* Запрашиваем информацию о портах
  
do sh port br

порт te0 обращен к ISP-HQ

порт te1 обращен к HQ-NET

* Создание нужных подинтерфейсов
  
Int isp

Ip address 172.16.4.14/28

Ex

* Следующие интерфейсы будем называть именами VLAN-ов
  
Int 100

Ip address 192.168.100.2/26

No shutdown

ex

* Int 200
  
Ip address 192.168.200.14/28

No shutdown

Ex

* Int  999
  
Ip address 192.168.99.14/28

No shutdown

Ex

* Теперь нужно связать интерфейсы с портами
  
port te0 - интерфейс isp

port te0 

service-instance isp 

encapsulation untagged 

connect ip interface isp 

ex 

порт te1 - интерфейсы 100, 200, 999

* для инт 100
  
port te1 

service-instance 100 

encapsulation dot1q 100 

rewrite pop 1 

connect ip interface 100 

Ex

* для инт 200
  
port te1

service-instance 200 

encapsulation dot1q 200 

rewrite pop 1 

connect ip interface 200 

ex  

* для инт 999
  
port te1 

service-instance 999

encapsulation dot1q 999 

rewrite pop 1 

connect ip interface 999 

ex 

* Прописываем маршрут по умолчанию
  
ip route 0.0.0.0/0 172.16.4.1 (маршрутизатор ISP)

* Временно назначаем DNS

ip name-server 77.88.8.8

* Настраиваем пул адресов nat
  
ip nat pool INTERNET 192.168.100.2-192.168.100.87

ip nat pool INTERNET 192.168.200.2-192.168.200.87

ip nat pool INTERNET 192.168.99.2-192.168.99.87

ip nat source dynamic inside-to-outside pool INTERNET overload 172.16.4.14

* Теперь необходимо пройтись по интерфейсам ipv4 (внутренний или внешний интерфейс)
  
int isp 

ip nat outside 

ex 

int 100 

ip nat inside 

ex 

int 200 

ip nat inside 

ex 

int 999 

ip nat inside 

ex 

# Задание 6. Между офисами HQ и BR необходимо сконфигурировать IP-туннель

1.3  Создаем туннель в HQ-RTR

Int tunnel.1 

Ip address 10.10.10.9/30 

Ip tunnel 172.16.4.14 172.16.5.14 mode gre 

Ex 

Do wr 

* Настройка ospf
  
En

Conf t

Router ospf 1

Network 10.10.10.8/30 area 0

Network 192.168.1.0/26 area 0

Network 192.168.1.64/28 area 0

Network 192.168.1.80/29 area 0

* Делаем пассивные интерфейсы
  
Passive-interface isp

Passive-interface 100

Passive-interface 200

Passive-interface 999

Area 0 authentication message-digest

Ex

# Задание 3. Создайте пользователя net_admin на маршрутизаторах HQ-RTR и BR-RTR.

Conf t

username net_admin 

password P@ssw0rd 

role admin

write

# Задание 7. Настройка OSPF на HQ-RTR

router ospf 1

  router-id 100.10.10.1
  
  network 192.168.100.0/26 area 0
  
  network 192.168.200.64/28 area 0
  
  network 192.168.99.0/29 area 0
  
  passive-interface default
  
  no passive-interface tunnel.0

# Задание 8. Настройка динамической трансляции адресов

Указываем внутренние и внешние интерфейсы:

int int1

  ip nat inside
  
!

int int2

  ip nat inside
  
!

int int0

  ip nat outside
  
* Вся настройка выше - задание 1.2

* Создаем туннель: задание 2
  
Проверка туннель

Ping 10.10.10.10

# Задание 9. Настройка протокола динамической конфигурации хостов

Ip pool hq 192.168.1.67-192.168.1.78

* Далее настраиваем сервер-dhcp
  
dhcp-server 1

static ip 192.168.1.66

client-id mac {mac-адрес клиента (HQ-CLI)} 000с.2935.554е

mask 255.255.255.240

gateway 192.168.1.65

dns 192.168.1.2

domain-search au-team.irpo

Exit

* dhcp-server 1
  
pool hq 1

mask 255.255.255.240

gateway 192.168.1.65

dns 192.168.1.2

domain-search au-team.irpo

Ex

* Подключаем dhcp-server к интерфейсу
  
int 200

dhcp-server 1

do wr

# Задание 11. Настройте часовой пояс на всех устройствах, согласно месту проведения экзамена

show ntp ?

Show ntp date

Ntp timezone utc+3

show ntp timezone

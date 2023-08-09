# Домашнее задание к занятию "4.10. DHCP, PXE"

---

### Задание 1. 

Для чего служит протокол `DHCP`? 

Может ли работать сеть без `DHCP-сервера`?

*Приведите ответ в свободной форме.*


**DHCP служит для автоматической выдачи временного IP-адреса и других параметров во время подключения устройств к сети. Сеть может работать без DHCP-сервера, если всем устройствам присвоить статический IP  и маску вручную.**

---

### Задание 2. 

На каком порту/портах работает `DHCP`? 

*Приведите ответ в свободной форме.*

*DHCP - это клиент-серверный протокол, использующий в качестве транспорта UDP, порт сервера - 67, порт клиента - 68. Так как для работы протокола используется широковещание (broadcast), то его действие ограничено текущей подсетью (широковещательным доменом). Для взаимодействия с клиентами расположенными в иных широковещательных доменах могут использоваться агенты ретрансляции (DHCP Relay)*

---

### Задание 3. 

Какие настройки можно произвести используя опции? 

Назовите 5.

*Приведите ответ в свободной форме.*

*1) Subnet Mask - Маска подсети*

*2) Router - IP-адреса доступных шлюзов по умолчанию, в сети их может быть несколько*

*3) Name Server - Перечисление доступных DNS серверов*

*4) Default IP TTL  - Позволяет задать TTL, которое обязан будет использовать клиент при отправке пакетов в сеть*

*5) Client Identifier - DHCP-сервер ведет сопоставление IP и MAC-адресов, он запоминает какому мак-адресу какой IP был назначен*


---

### Задание 4. 

Сконфигурируйте сервер `DHCP`.

*Пришлите получившийся конфигурационный файл.*

<ins>Ответ</ins>:
*Установка сервера:*
```
sudo apt install isc-dhcp-server 
```
*Настройки в файле конфигурации /etc/dhcp/dhcpd.conf*
```
sudo nano /etc/dhcp/dhcpd.conf
```
```
subnet 192.168.0.0 netmask 255.255.255.0 {

  range 192.168.0.2 192.168.0.254;

  option domain-name-servers 8.8.8.8;

  option domain-name "demin-net.local";

  option routers 192.168.0.1;

  default-lease-time 600;

  max-lease-time 7200;

}
```
*Настройки в файле конфигурации /etc/default/isc-dhcp-server:*
```
# Defaults for isc-dhcp-server (sourced by /etc/init.d/isc-dhcp-server)


# Path to dhcpd's config file (default: /etc/dhcp/dhcpd.conf).

DHCPDv4_CONF=/etc/dhcp/dhcpd.conf

#DHCPDv6_CONF=/etc/dhcp/dhcpd6.conf


# Path to dhcpd's PID file (default: /var/run/dhcpd.pid).

#DHCPDv4_PID=/var/run/dhcpd.pid

#DHCPDv6_PID=/var/run/dhcpd6.pid


# Additional options to start dhcpd with.

# Don't use options -cf or -pf here; use DHCPD_CONF/ DHCPD_PID instead

#OPTIONS=""

# On what interfaces should the DHCP server (dhcpd) serve DHCP requests?

# Separate multiple interfaces with spaces, e.g. "eth0 eth1".

INTERFACESv4="enp0s3"

INTERFACESv6=""
```
*Рестарт сервиса:*
```
sudo systemctl restart isc-dhcp-server
```
*Просмотр статуса:*
```
sudo systemctl status isc-dhcp-server
```
*Если сервер не стартовал, смотрим журнал:*
```
sudo journalctl --unit isc-dhcp-server
```
*В моем случае проблема решена удалением файла процесса:*
```
sudo rm /var/run/dhcpd.pid
```
```
● isc-dhcp-server.service - LSB: DHCP server

     Loaded: loaded (/etc/init.d/isc-dhcp-server; generated)

     Active: active (running) since Wed 2023-08-09 20:19:39 +06; 1min 7s ago

       Docs: man:systemd-sysv-generator(8)

    Process: 5667 ExecStart=/etc/init.d/isc-dhcp-server start (code=exited, status=0/SUCCESS)

      Tasks: 12 (limit: 2316)

     Memory: 17.2M

        CPU: 36ms

     CGroup: /system.slice/isc-dhcp-server.service

             ├─4438 /usr/sbin/dhcpd -4 -q -cf /etc/dhcp/dhcpd.conf

             ├─5572 /usr/sbin/dhcpd -4 -q -cf /etc/dhcp/dhcpd.conf enp0s3

             └─5683 /usr/sbin/dhcpd -4 -q -cf /etc/dhcp/dhcpd.conf enp0s3



авг 09 20:19:37 DEBIAN-11x64 systemd[1]: Starting LSB: DHCP server...

авг 09 20:19:37 DEBIAN-11x64 isc-dhcp-server[5667]: Launching IPv4 server only.

авг 09 20:19:37 DEBIAN-11x64 dhcpd[5683]: Wrote 1 leases to leases file.

авг 09 20:19:37 DEBIAN-11x64 dhcpd[5683]: Server starting service.

авг 09 20:19:39 DEBIAN-11x64 isc-dhcp-server[5667]: Starting ISC DHCPv4 server: dhcpd.

авг 09 20:19:39 DEBIAN-11x64 systemd[1]: Started LSB: DHCP server.

```
*Настройка файла конфигурации сетевого адаптера клиентского хоста:*
```
sudo nano /etc/network/interfaces
```
```
auto enp0s3

allow-hotplug enp0s3

iface enp0s3 inet dhcp
```
*Рестарт сетевого адаптера клиентского хоста для применения настроек:*
```
sudo /etc/init.d/networking restart
```
---

## Дополнительные задания (со звездочкой*)
Эти задания дополнительные (не обязательные к выполнению) и никак не повлияют на получение вами зачета по этому домашнему заданию. Вы можете их выполнить, если хотите глубже и/или шире разобраться в материале.



### Задание 5. 

Поймайте в сети пакеты `DHCP` любым сниффером. 

*Пришлите скриншот пойманого одного пакета с объяснением что это за пакет, какой шаг получения сетевых настроек.*

---

### Задание 6. 

Сконфигурируйте сервер `PXE`, выложите любой образ. 

*Пришлите скриншот клиента, который получил настройки и подключился к `PXE-серверу`.*

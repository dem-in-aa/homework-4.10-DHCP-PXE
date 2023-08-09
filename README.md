# Домашнее задание к занятию "4.10. DHCP, PXE"

---

### Задание 1. 

Для чего служит протокол `DHCP`? 

Может ли работать сеть без `DHCP-сервера`?

*Приведите ответ в свободной форме.*


*DHCP служит для автоматической выдачи временного IP-адреса и других параметров во время подключения устройств к сети. Сеть может работать без DHCP-сервера, если всем устройствам присвоить статический IP  и маску вручную.*

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

*Subnet Mask - Маска подсети
Router - IP-адреса доступных шлюзов по умолчанию, в сети их может быть несколько
Name Server - Перечисление доступных DNS серверов
Default IP TTL  - Позволяет задать TTL, которое обязан будет использовать клиент при отправке пакетов в сеть
Client Identifier - DHCP-сервер ведет сопоставление IP и MAC-адресов, он запоминает какому мак-адресу какой IP был назначен*


---

### Задание 4. 

Сконфигурируйте сервер `DHCP`.

*Пришлите получившийся конфигурационный файл.*

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

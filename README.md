# Разворачиваем сетевую лабораторию<br><br>
Описание домашнего задания<br><br>
построить следующую архитектуру<br>
Сеть office1<br>
•	192.168.2.0/26 - dev<br>
•	192.168.2.64/26 - test servers<br>
•	192.168.2.128/26 - managers<br>
•	192.168.2.192/26 - office hardware<br><br>
Сеть office2<br>
•	192.168.1.0/25 - dev<br>
•	192.168.1.128/26 - test servers<br>
•	192.168.1.192/26 - office hardware<br><br>
Сеть central<br>
•	192.168.0.0/28 - directors<br>
•	192.168.0.32/28 - office hardware<br>
•	192.168.0.64/26 - wifi<br><br>
```
Office1 ---\
             -----> Central --IRouter --> internet
Office2----/
```
Итого должны получится следующие сервера<br>
•	inetRouter<br>
•	centralRouter<br>
•	office1Router<br>
•	office2Router<br>
•	centralServer<br>
•	office1Server<br>
•	office2Server<br><br>

Теоретическая часть<br>
•	Найти свободные подсети<br>
•	Посчитать сколько узлов в каждой подсети, включая свободные<br>
•	Указать broadcast адрес для каждой подсети<br>
•	проверить нет ли ошибок при разбиении<br><br>

Практическая часть<br>
•	Соединить офисы в сеть согласно схеме и настроить роутинг<br>
•	Все сервера и роутеры должны ходить в инет черз inetRouter<br>
•	Все сервера должны видеть друг друга<br>
•	у всех новых серверов отключить дефолт на нат (eth0), который вагрант поднимает для связи<br>
•	при нехватке сетевых интервейсов добавить по несколько адресов на интерфейс
______________________________________________________________________________________________

Целевая схема сети:<br><br>
<img width="675" height="785" alt="image" src="https://github.com/user-attachments/assets/59429bcd-cb0a-4e25-be14-b8efd2f36d64" /><br><br>
В исходный vagrantfile добавляем необходимые сервера. Поднимаем стенд из получившегося Vagrantfile (ССЫЛКА) <br><br>
В силу особенностей архитектуры ВМ с Ansible поднималась так же из Vagrant, и плейбуки писались/дебажились внутри этой ВМ. В текущем Vagrantfile она закомментирована, плейбуки лежат отдельно:<br>


После поднятия стенда запускаем плейбук "setup_routers.yml", затем "save_netplan.yml".<br><br>
В итоге имеем 8 ВМ: ansible + 7шт по заданию<br><br>
<img width="624" height="629" alt="image" src="https://github.com/user-attachments/assets/f0f3b144-7852-49ad-8e3f-fdf1720dcc2f" /><br><br>
Сделаем выборочные проверки связности.<br><br>
Интернет на centralServer: <br><br>
<img width="1035" height="600" alt="image" src="https://github.com/user-attachments/assets/68db931f-3285-4eb6-8999-f37374be2aa9" /><br><br>
Интернет на office1Server + связность с office2Server: <br><br>
<img width="1057" height="364" alt="image" src="https://github.com/user-attachments/assets/725e7787-fb2a-40bf-99b5-74c9fcda28b1" /><br><br>
Интернет на office2Server + связность с centralServer: <br><br>
<img width="980" height="317" alt="image" src="https://github.com/user-attachments/assets/25bfe3dd-914c-433d-a864-75d09d4f0d1a" />

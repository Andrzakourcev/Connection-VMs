# Установка VirtualBox

Для начала я установила VirtualBox. Дальше я скачала виртуальный образ iso из интернета с версией ubuntu 22.04

# Настройка машины A. Доступ в интеренет.

Создадим машину A. Заходим в настройки машины А, потом переходим во вкладку сеть. Чтобы у нас был доступен интернет, нам необходимо выбрать NAT подключение. NAT изолирует нашу машину от соединений вне локальной сети. Все запросы проходят через хост-систему.

![image](https://github.com/Andrzakourcev/Hello-world/assets/144477949/a5e3ab19-75e3-4235-b4fc-887f34920f48)


# Проверка доступа в интернет

Используем команду ping, она осуществляет обмен пакетами по ip-адресу. Возьмем, например, ip-адрес гугла - 216.58.209.164

![image](https://github.com/Andrzakourcev/Hello-world/assets/144477949/71dff31f-7d04-470a-856c-c893521ee1ea)


Подключение работает. Все пакеты дошли.

# Настройка машины B.

Создадим машину B из того же образа. Заходим в настройки, опять же выбираем сеть. Кликаем на 1 адаптер, тип подключения - Внутренняя сеть. Выбираем имя - loc1. Те же действия проводим с машиной A, но уже на втором адаптере, так как первый у нас занят NAT подключением. 

![image](https://github.com/Andrzakourcev/Hello-world/assets/144477949/0a23f23e-68fc-479d-ad7a-de06102d1995)

![image](https://github.com/Andrzakourcev/Hello-world/assets/144477949/4879272f-a9b2-493a-bf3d-55721eb80aec)


# Подключение машин A и B
Команда «ip» используется для настройки параметров сетевого интерфейса и назначения ему адреса в ОС Linux.

В терминале A пишем: 
```
ip a
```
![image](https://github.com/Andrzakourcev/Hello-world/assets/144477949/f4891cad-9597-4bd0-8119-87f0f22de5bf)

Видим несколько подключений. Первое - lo. Оно есть на любом устройстве, его ip всегда 127.0.0.1. Это способ обратиться к самому себе же. enp0s3 - это наша сеть для выхода в интернет с помощью NAT, enp0s8 - локальная сеть между машинами А и B. Дальше с помощью команды ip мы добавим порт с нужным адресом.

```

sudo ip addr add 192.168.1.1/255.255.255.0 dev enp0s8

```
![image](https://github.com/Andrzakourcev/Hello-world/assets/144477949/7d88a3f8-13c8-41ac-873d-44a73d9378cf)

То есть мы добавили ip-адрес с маской локальной сети, для интернет соединения enp0s8, 192.168 - локальная сеть, .1 - адрес подсети, .1 - номер машины A в сети.

Теперь на машине B, но меняем номер машины в подсети на второй - 192.168.1.2, а enp0s8 на enp0s3. Так же используем команду ip.

![image](https://github.com/Andrzakourcev/Hello-world/assets/144477949/953f514d-07d9-40c7-9e2c-975fe4b380f6)

# Проверка соединения машин A и B

Опять же используем команду ping

На машине A

```
ping 192.168.1.2
```

![image](https://github.com/Andrzakourcev/Hello-world/assets/144477949/1b6fd891-d6f1-47bc-bfda-b2a9f91d5d83)

На машине B

```
ping 192.168.1.1
```
![image](https://github.com/Andrzakourcev/Hello-world/assets/144477949/347e0995-c1e5-45e0-8131-8eeab0c06fb7)

Подключение работает от A к B и наоборот, отлично!

# Подключение машин A и С

Создадим машину C. Теперь для машины A в настройках сети выберем 3 адаптер, подключаемся так же с помощью внутренней сети, однако название уже loc2.

![image](https://github.com/Andrzakourcev/Hello-world/assets/144477949/a6ca03f8-1299-4a9c-ab43-37435b3ccccf)

У машины C в 1 адаптере выбираем ту же сеть loc2.

![image](https://github.com/Andrzakourcev/Hello-world/assets/144477949/a1c22306-336b-4a25-b6eb-48f648d4e95c)


Теперь все то же самое, как и в первом случае, однако мы должны подменить номер подсети, так нам ip для машины A - 192.168.2.1, для C - 192.168.2.2

Добавляем порт с нужным ip в машине A для enp0s9.


![image](https://github.com/Andrzakourcev/Hello-world/assets/144477949/beefe83c-6322-4111-8065-4a0e70d7be7d)



Для машины C:

![image](https://github.com/Andrzakourcev/Hello-world/assets/144477949/49af3079-81b3-49c3-be08-7d2138b57482)


# Проверка работы A и C
Используем ping

Машина A:

![image](https://github.com/Andrzakourcev/Hello-world/assets/144477949/e4f8667f-b72d-4fd5-8d9a-0582c34966a5)


Машина С:

![image](https://github.com/Andrzakourcev/Hello-world/assets/144477949/33c2992b-41af-4a25-9f56-2e8aed43251c)

Все работает.

# Проверка отсутствия соединения между B и C
Попробуем проверить соединение между B и С. Используем ping

От B к C:

![image](https://github.com/Andrzakourcev/Hello-world/assets/144477949/2e16afc2-9e8f-4dbb-ae02-1062e72a4b56)


От C к B:

![image](https://github.com/Andrzakourcev/Hello-world/assets/144477949/05e99aad-9a7d-46ab-a8a7-217a9f594ab9)


Подключение отсутствует.

# Скриншот всех 3 терминалов:

![image](https://github.com/Andrzakourcev/Hello-world/assets/144477949/9304cfba-a6a8-41b3-88f7-a934b2bdd5c9)



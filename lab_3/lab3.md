University: [ITMO University](https://itmo.ru/ru/)

Faculty: [FICT](https://fict.itmo.ru/)

Course: [Introduction to distributed technologies](https://github.com/itmo-ict-faculty/introduction-to-distributed-technologies)

Year: 2024/2025

Group: K4111c

Author: Kovalev Evgenii Kovalev

Lab: Lab1

Date of create: 15.12.2025

Date of finished: 16.12.2025

# Лабораторная работа №3
"Сертификаты и "секреты" в Minikube, безопасное хранение данных."

## Цель работы 
Познакомиться с сертификатами и "секретами" в Minikube, правилами безопасного хранения данных в Minikube.

## Ход работы
Создадим ConfigMap c переменными REACT_APP_USERNAME и REACT_APP_COMPANY_NAME
<img width="505" alt="image" src="https://github.com/user-attachments/assets/2e87889c-2ed0-4d70-a294-9773ea266299" />

Cоздадим конфигурацию ReplicaSet
<img width="517" alt="image" src="https://github.com/user-attachments/assets/bdd2ab1b-02f5-483f-a393-cf2b1a7c9c27" />

Для работы с Ingress включаем нужным нам аддон.
```minikube addons enable ingress```

Сгенерируем приватный ключ, а также запрос на подпись сертификата:
```
openssl genrsa -out evgevg.key 4096
openssl req -key evgevg.key -new -out evgevg.csr
```

Подпишем сертификат:
```openssl x509 -signkey evgevg.key -in evgevg.csr -req -days 365 -out evgevg.crt```

Cоздадим для него Secret в minikube:
```kubectl create secret tls evgevgevg-tls --cert=evgevg.crt --key=evgevg.key```

Используем его при конфигурировании Ingress:
<img width="519" alt="image" src="https://github.com/user-attachments/assets/bc39af1f-98b9-4275-8188-1193db54aa62" />

<img width="632" alt="image" src="https://github.com/user-attachments/assets/318a2751-32af-4f70-a602-8c9057a5f0cf" />

Чтобы иметь возможность заходить по FQDN, необходимо добавить запись о нем в ```/etc/hosts```
<img width="697" alt="image" src="https://github.com/user-attachments/assets/9aa57182-7f1e-4448-bdfc-570786cd4e32" />

Схема:
<img width="627" alt="image" src="https://github.com/user-attachments/assets/e5fd71bd-b17d-42de-b089-7f10eac841aa" />


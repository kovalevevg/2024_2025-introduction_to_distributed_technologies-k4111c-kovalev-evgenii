University: [ITMO University](https://itmo.ru/ru/)

Faculty: [FICT](https://fict.itmo.ru/)

Course: [Introduction to distributed technologies](https://github.com/itmo-ict-faculty/introduction-to-distributed-technologies)

Year: 2024/2025

Group: K4111c

Author: Kovalev Evgenii Kovalev

Lab: Lab1

Date of create: 15.12.2025

Date of finished: 16.12.2025

# Лабораторная работа №4
"Сети связи в Minikube, CNI и CoreDNS"

## Цель работы 
Познакомиться с CNI Calico и функцией IPAM Plugin, изучить особенности работы CNI и CoreDNS.

## Ход работы
Запускается minikube с подключенным плагином CNI=calico, режимом работы Multi-Node Clusters и разворачиваются 2 ноды с помощью команды:
<img width="815" alt="image" src="https://github.com/user-attachments/assets/8bbcc821-c791-4b53-85c1-bf9828205d79" />

Проверим, запустились ли 2 Node:
<img width="457" alt="image" src="https://github.com/user-attachments/assets/9c92402a-440b-4c04-b493-79aa31169040" />

Проверим работу CNI Calico. Их число должно совпадать с количеством нод. Проверяем с помощью следующей команды:
<img width="568" alt="image" src="https://github.com/user-attachments/assets/7ab735e9-8ac1-4937-b125-000eefca67f9" />

Удалим ippool
```ippool.crd.projectcalico.org "default-ipv4-ippool" deleted```

```kubectl label node minikube location=east```

```kubectl label node minikube location=west```

Создим deployment с 2 репликами контейнера ifilyaninitmo/itdt-contained-frontend:master и передать переменные в эти реплики: REACT_APP_USERNAME, REACT_APP_COMPANY_NAME.

Применим манифест
```kubectl apply -f deployment.yaml```

Создать сервис, через который будет доступ на эти "поды"
```kubectl apply -f service.yaml```

Запустить в minikube режим проброса портов и подключитесь к контейнерам через веб браузер
```minikube kubectl -- port-forward service/frontend-service-4 3000:3000```

Проверить на странице в веб браузере переменные Container name и Container IP.
<img width="688" alt="image" src="https://github.com/user-attachments/assets/da8cac60-7f72-45a6-a9b2-536354356d20" />

Проверяем, какие поды у нас есть kubectl ```get pods -o wide```
```
NAME                              READY   STATUS    RESTARTS   AGE   IP             NODE           NOMINATED NODE   READINESS GATES
lab4-6f76fc4f58-hsfwz   1/1     Running   0          13m   192.168.42.9    minikube-m02   <none>           <none>
lab4-6f76fc4f58-hsfwz   1/1     Running   0          13m   192.168.1.0     minikube       <none>           <none>
```




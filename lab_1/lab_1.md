University: [ITMO University](https://itmo.ru/ru/)
Faculty: [FICT](https://fict.itmo.ru/)
Course: [Introduction to distributed technologies](https://github.com/itmo-ict-faculty/introduction-to-distributed-technologies)
Year: 2024/2025
Group: K4111c
Author: Kovalev Evgenii Kovalev
Lab: Lab1
Date of create: 7.12.2025\n
Date of finished: TBD

# Лабораторная работа №1

"Установка Docker и Minikube, мой первый манифест."

## Цель работы

Ознакомиться с инструментами Minikube и Docker, развернуть свой первый первый манифест."

## Ход работы
«Для успешного выполнения необходимо, чтобы в системе были установлены docker, kubectl и minikube.»

Запускаем minikube при помощи команды:

```minikube start --driver=docker```
<img width="775" alt="Снимок экрана 2025-01-07 в 13 49 29" src="https://github.com/user-attachments/assets/c3f1f05e-80c1-4509-b018-266597283c3d" />

lab_1.yaml
```
apiVersion: v1
kind: Pod
metadata:
  name: vault
  labels:
    name: vault
spec:
  containers:
  - name: vault
    image: vault:1.13.3
    ports:
      - containerPort: 8200
```
Применяем манифест
```minikube kubectl -- apply -f lab_1.yaml```
<img width="562" alt="Снимок экрана 2025-01-07 в 14 09 14" src="https://github.com/user-attachments/assets/5c04ad04-3ead-4493-9fea-8e0b420103bc" />


Создаем кластер, который доступен из вне 
```minikube kubectl -- expose pod vault --type=NodePort --port=8200```
<img width="723" alt="Снимок экрана 2025-01-07 в 14 09 49" src="https://github.com/user-attachments/assets/bc862e2e-ffac-49bc-b5c5-69a6c52e6da5" />

Прокидываем порт компьютера в контейнере, чтобы попасть по [ссылке(http://localhost:8200/ui/vault/secrets)
```minikube kubectl -- port-forward service/vault 8200:8200```
<img width="701" alt="Снимок экрана 2025-01-07 в 14 14 15" src="https://github.com/user-attachments/assets/a3b2e0ce-1bbb-4524-b9a0-61efb9865348" />

Чтобы найти токен для авторизации, введем команду
```kubectl logs vault```
в которой найдем токен
<img width="1728" alt="Снимок экрана 2025-01-07 в 14 17 35" src="https://github.com/user-attachments/assets/a4420f1d-1c51-49dd-a8fd-1f91d18fa217" />

## Схема
<img width="498" alt="Снимок экрана 2025-01-08 в 17 23 41" src="https://github.com/user-attachments/assets/fb1ae0ba-7a76-441d-af37-1d1736222267" />


## Вывод
В ходе лабораторной работы мы познакомились с инструментами Minikube и Docker, а также развернули свой Vault под.

## Ответы на вопросы 
1. Что сейчас произошло и что сделали команды указанные ранее?
   В кубере-кластера minikube был создан под с контейнером, в котором было запущено хранилище
   Vault. Для связи с сервисом был прокинут порт 8200 у локального хоста через такой же порт
   у контейнера.

2. Где взять токен для входа в Vault?
   В логах пода

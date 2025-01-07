University: ITMO University
Faculty: FICT
Course: Introduction to distributed technologies
Year: 2024/2025
Group: K4111c
Author: Kovalev Evgenii Kovalev
Lab: Lab1
Date of create: 7.12.2025
Date of finished: TBD

# Лабораторная работа №1

"Установка Docker и Minikube, мой первый манифест."

## Цель работы

Ознакомиться с инструментами Minikube и Docker, развернуть свой первый первый манифест."

## Ход работы
«Для успешного выполнения необходимо, чтобы в системе были установлены docker, kubectl и minikube.»

Запускаем minikube при помощи команды:

```minikube start --driver=docker```

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
```kubectl apply -f lab_1.yaml```

Создаем кластер, который доступен из вне 
```minikube kubectl -- expose pod vault --type=NodePort --port=8200```

Прокидываем порт компьютера в контейнере, чтобы попасть по [ссылке(http://localhost:8200/ui/vault/secrets)
```minikube kubectl -- port-forward service/vault 8200:8200```

Чтобы найти токен для авторизации, введем команду
```kubectl logs vault```
в которой найдем токен

## Вывод
В ходе лабораторной работы мы познакомились с инструментами Minikube и Docker, а также развернули свой Vault под.

## Ответы на вопросы 
1. Что сейчас произошло и что сделали команды указанные ранее?
   В кубере-кластера minikube был создан под с контейнером, в котором было запущено хранилище
   Vault. Для связи с сервисом был прокинут порт 8200 у локального хоста через такой же порт
   у контейнера.

2. Где взять токен для входа в Vault?
   В логах пода

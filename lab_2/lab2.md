# Цель работы 
Ознакомиться с типами "контроллеров" развертывания контейнеров, ознакомится с сетевыми сервисами и развернуть свое веб приложение.

# Ход работы
Создади м конфигурацию deployment с 2 репликами контейнера ifilyaninitmo/itdt-contained-frontend:master, в которые передадим переменные REACT_APP_USERNAME и REACT_APP_COMPANY_NAME:
'''
apiVersion: apps/v1
kind: Deployment
metadata:
  name: lab2-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: lab2-deployment
  template:
    metadata:
      labels:
        app: lab2-deployment
    spec:
      containers:
      - name: deployment
        image: ifilyaninitmo/itdt-contained-frontend:master
        ports:
        - containerPort: 3000
        env:
          - name: REACT_APP_USERNAME
            value: "Kovalev Evgenii"
          - name: REACT_APP_COMPANY_NAME
            value: "evkovalev"
'''

Добавим следом конфигурацию сервиса:

'''
apiVersion: v1
kind: Service
metadata:
  name: frontend-service
spec:
  selector:
    app: frontend
  ports:
  - protocol: TCP
    port: 80
    targetPort: 3000
  type: NodePort 
'''

Применим менифест и развертывание:
'''minikube kubectl -- apply -f lab2-deployment.yaml'''
'''minikube kubectl -- apply -f service.yaml'''
<img width="616" alt="image" src="https://github.com/user-attachments/assets/f4782a27-edd9-400d-95c2-81d9a7143d9f" />

Проверим состояние:
<img width="595" alt="image" src="https://github.com/user-attachments/assets/4f110098-85dc-47a3-8cf0-5e61d1eaaf5b" />

Создаем сервис доступный внутри кластера:
<img width="871" alt="image" src="https://github.com/user-attachments/assets/f387d983-033c-4e47-84a9-fc82f69cd35b" />

Пробрасываем порт:
<img width="777" alt="image" src="https://github.com/user-attachments/assets/4a2c49f9-6937-47ec-9050-7fe36f10c739" />

Переходим по адресу: http://localhost:3000
<img width="857" alt="image" src="https://github.com/user-attachments/assets/78874b80-9109-4d39-9d9b-ae8e9cf9ca57" />

Проверяем логи:
<img width="667" alt="image" src="https://github.com/user-attachments/assets/3c5ac99f-4137-4acb-aa84-36c046fe122a" />


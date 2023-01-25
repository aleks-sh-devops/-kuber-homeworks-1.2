# Домашнее задание к занятию "Базовые объекты K8S"

### Цель задания

В тестовой среде для работы с Kubernetes, установленной в предыдущем ДЗ, необходимо развернуть Pod с приложением и подключиться к нему со своего локального компьютера. 

### Задание 1. Создать Pod с именем "hello-world"

1. Создать манифест (yaml-конфигурацию) Pod
2. Использовать image - gcr.io/kubernetes-e2e-test-images/echoserver:2.2
3. Подключиться локально к Pod с помощью `kubectl port-forward` и вывести значение (curl или в браузере)

### Ответ

Манифест 01-pod-01.yaml:  
```
apiVersion: v1
kind: Pod
metadata:
  labels:
    app: netology-web
  name: netology-web
spec:
  containers:
    - image: gcr.io/kubernetes-e2e-test-images/echoserver:2.2
      name: echoserver
```

Проброс:  
```
kubectl port-forward pods/echoserver01 8081:8080
Forwarding from 127.0.0.1:8081 -> 8080
Forwarding from [::1]:8081 -> 8080
Handling connection for 8081
Handling connection for 8081
```
curl:  
```
curl -l http://127.0.0.1:8081

Hostname: echoserver01

Pod Information:
        -no pod information available-

Server values:
        server_version=nginx: 1.12.2 - lua: 10010

Request Information:
        client_address=127.0.0.1
        method=GET
        real path=/
        query=
        request_version=1.1
        request_scheme=http
        request_uri=http://127.0.0.1:8080/

Request Headers:
        accept=*/*
        host=127.0.0.1:8081
        user-agent=curl/7.81.0

Request Body:
        -no body in request-

```
------

### Задание 2. Создать Service и подключить его к Pod

1. Создать Pod с именем "netology-web"
2. Использовать image - gcr.io/kubernetes-e2e-test-images/echoserver:2.2
3. Создать Service с именем "netology-svc" и подключить к "netology-web"
4. Подключиться локально к Service с помощью `kubectl port-forward` и вывести значение (curl или в браузере)

### Ответ

Манифест 01-pod-01.yaml:  
```
apiVersion: v1
kind: Service
metadata:
  name: netology-svc
spec:
  ports:
    - name: netology
      port: 8080
  selector:
    app: netology-web
```
Эндпоинты и сервисы:  
```
kubectl get svc,ep

NAME                   TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)    AGE
service/kubernetes     ClusterIP   10.152.183.1     <none>        443/TCP    7d11h
service/netology-svc   ClusterIP   10.152.183.121   <none>        8080/TCP   19h

NAME                     ENDPOINTS            AGE
endpoints/kubernetes     10.50.41.138:16443   7d11h
endpoints/netology-svc   10.1.174.16:8080     19h
```
Проброс:  
```
kubectl port-forward svc/netology-svc 10080:8080
Forwarding from 127.0.0.1:10080 -> 8080
Forwarding from [::1]:10080 -> 8080
Handling connection for 10080
```
curl:  
```
curl -l http://127.0.0.1:10080

Hostname: netology-web

Pod Information:
        -no pod information available-

Server values:
        server_version=nginx: 1.12.2 - lua: 10010

Request Information:
        client_address=127.0.0.1
        method=GET
        real path=/
        query=
        request_version=1.1
        request_scheme=http
        request_uri=http://127.0.0.1:8080/

Request Headers:
        accept=*/*
        host=127.0.0.1:10080
        user-agent=curl/7.81.0

Request Body:
        -no body in request-
```

ПОДы:
```
kubectl get pods

NAME           READY   STATUS    RESTARTS   AGE
echoserver01   1/1     Running   0          20h
web11          1/1     Running   0          20h
netology-web   1/1     Running   0          19h
init-pod       1/1     Running   0          14h
```
------

### Правила приема работы

1. Домашняя работа оформляется в своем  Git репозитории в файле README.md. Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.
2. Файл README.md должен содержать скриншоты вывода команд `kubectl get pods`, а также скриншот результата подключения
3. Репозиторий должен содержать файлы манифестов и ссылки на них в файле README.md

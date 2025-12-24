 Вывод команды kubectl get all -n task-manager

 [aoizora@archlinux task-manager]$ kubectl get all -n task-manager
NAME                                       READY   STATUS    RESTARTS   AGE
pod/backend-deployment-cfbd5d7c8-j6nd7     1/1     Running   0          4h9m
pod/backend-deployment-cfbd5d7c8-k8r4q     1/1     Running   0          4h9m
pod/backend-deployment-cfbd5d7c8-wrqpz     1/1     Running   0          4h9m
pod/frontend-deployment-7bcb976b5c-7f62p   1/1     Running   0          4h15m
pod/network-test                           1/1     Running   0          11m

NAME                       TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
service/backend-service    ClusterIP      10.108.25.202   <none>        8080/TCP       21m
service/frontend-service   LoadBalancer   10.96.118.105   <pending>     80:30080/TCP   22m

NAME                                  READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/backend-deployment    3/3     3            3           4h15m
deployment.apps/frontend-deployment   1/1     1            1           4h15m

NAME                                             DESIRED   CURRENT   READY   AGE
replicaset.apps/backend-deployment-5d56c6cf5c    0         0         0       4h15m
replicaset.apps/backend-deployment-cfbd5d7c8     3         3         3       4h9m
replicaset.apps/frontend-deployment-7bcb976b5c   1         1         1       4h15m
[aoizora@archlinux task-manager]$ 


Результат тестирования сетевой связности между сервисами

/ # wget -qO- http://backend-service.task-manager.svc.cluster.local:8080
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
<html>
<head>
<title>It works! Apache httpd</title>
</head>
<body>
<p>It works!</p>
</body>
</html>
/ # wget -qO- http://frontend-service.task-manager.svc.cluster.local:80
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
/ # exit


Объяснение разницы между Pod'ами и Deployment'ами

Pod - единица развертывания, обертка для одного или нескольких контейнеров.
Deployment - менеджер и контроллер для Pod'ов. Он управляет их жизненным циклом, обеспечивает масштабируемость и отказоустойчивость

Объяснение назначения различных типов Service'ов

ClusterIP - недоступен снаружи, используется для общения подов друг с другом
NodePort - доступен снаружи, используется для общения с подом извне
LoadBalancer - для предоставления внешнего доступа к приложениям извне кластера через выделенный внешний IP-адрес, который автоматически распределяет входящий трафик между Pod'ами приложения
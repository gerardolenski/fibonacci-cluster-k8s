# Fibonacci on k8s
![](fibonacci-k8s.png)

## Running on microk8s

0. The microk8s was installed using snap
```
$ snap info microk8s
$ sudo snap install microk8s --classic --channel=1.30/stable
$ sudo usermod -a -G microk8s $USER
```

1. Start microk8s
```
$ mickrok8s start
```

2. We need to enable several addons
```
$ microk8s enable ingress
$ microk8s enable storage
$ microk8s enable dns
$ microk8s enable host-access
```

3. Deploy all objects located in `fib` directory to k8s cluster ()
```
$ microk8s kubectl apply -f fib/
```

Then verify all pods status in `fibonacci` namespace:
```
$ microk8s kubectl -n fibonacci get pods
NAME                        READY   STATUS    RESTARTS   AGE
front-67fcf8cbdd-hvmlr      1/1     Running   0          3m36s
front-67fcf8cbdd-thz7g      1/1     Running   0          3m36s
artemis-7cd79647d4-6s4r5    1/1     Running   0          3m36s
worker-66947dc44c-d9jtr     1/1     Running   0          3m35s
worker-66947dc44c-sj5xc     1/1     Running   0          3m35s
worker-66947dc44c-dv89r     1/1     Running   0          3m35s
pgadmin-7b5c945d8f-27n7j    1/1     Running   0          3m36s
postgres-64ccbdf9b4-dqqjw   1/1     Running   0          3m36s
manager-9c95f4c64-8fpf8     1/1     Running   5          3m36s
manager-9c95f4c64-f556t     1/1     Running   5          3m36s
```

4. Now the services are available on cluster ip, e.g:

- [http://10.0.0.1](http://10.0.0.1) - the Fibonacci app
- [http://10.0.0.1:31000](http://10.0.0.1:31000) - the PG admin portal (user: postgres@postgres.dev, password: postgres)
- [http://10.0.0.1:31001](http://10.0.0.1:31001) - the Artemis console (user: artemis, password:artemis)

## Running on minikube

1. Start minikube
```
$ minikube start
```

2. Be sure the INGRESS support is enable
```
$ minikube addons enable ingress
```

3. Deploy all objects located in `fib` directory to k8s cluster ()
```
$ kubectl apply -f fib/
```

Then verify all pods status in `fibonacci` namespace:
```
$ kubectl -n fibonacci get pods
NAME                       READY   STATUS    RESTARTS   AGE
artemis-7cd79647d4-t5jgt   1/1     Running   0          2m24s
front-67fcf8cbdd-4f7wx     1/1     Running   0          2m24s
front-67fcf8cbdd-7chhs     1/1     Running   0          2m24s
manager-8bc9bbcc7-2mv9v    1/1     Running   0          2m24s
manager-8bc9bbcc7-s6tx9    1/1     Running   0          2m24s
pgadmin-59b695468c-8h66p   1/1     Running   0          2m24s
postgres-64489779b-bhmfk   1/1     Running   0          2m24s
worker-65dfdcc58-4kqqz     1/1     Running   0          2m24s
worker-65dfdcc58-86mzs     1/1     Running   0          2m24s
worker-65dfdcc58-vnsz4     1/1     Running   0          2m24s
```

4. Now the services are available on cluster ip, e.g:
```
$ minikube ip
192.168.70.2
```
- `http://192.168.70.2` - the Fibonacci app
- `http://192.168.70.2:31000` - the PG admin portal (user: postgres, password: qwerty)
- `http://192.168.70.2:31001` - the Artemis console (user: artemis, password:artemis)
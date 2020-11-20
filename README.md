# Fibonacci on k8s
![](fibonacci-k8s.png)
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

4. Create postgres database (only once)
   
During first start, the manager pods will get error because database does not exist. To fix this error, run this command once (the postgres pod must be available):
```
kubectl -n fibonacci exec \
  $(kubectl -n fibonacci get pod | grep postgres | sed -e 's/\s.*$//') \
  --container=postgres -- sh -c 'psql -U "$POSTGRES_USER" -c "create database task_manager"'
```

Then verify all pods status in `fibonacci` namespace:
```
$ kubectl -n fibonacci get pods
NAME                       READY   STATUS    RESTARTS   AGE
amq-7cd79647d4-t5jgt       1/1     Running   0          2m24s
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

5. Now the services are available on cluster ip, e.g:
```
$ minikube ip
192.168.70.2
```
- `http://192.168.70.2` - the Fibonacci app
- `http://192.168.70.2:31000` - the PG admin portal (user: postgres, password: qwerty)
- `http://192.168.70.2:31001` - the Active MQ console (user: admin, password:admin)
```
minikube addons enable ingress
```
```
kubectl -n fibonacci exec \
  $(kubectl -n fibonacci get pod | grep postgres | sed -e 's/\s.*$//') \
  --container=postgres -- sh -c 'psql -U "$POSTGRES_USER" -c "create database task_manager"'
```
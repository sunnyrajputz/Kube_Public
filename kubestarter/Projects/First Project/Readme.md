# 1. Clone django-notes-app
```bash
git clone 
```







**Create namespace mysql**
```bash
kubectl create ns mysql
```

```bash
kubectl apply -f service.yml && -f statefulsets.yml
```
# Copy any pod name 
```bash
kubectl get po -n mysql
```

```bash
kubectl exec -it <pod_name> -n mysql -- bash
```

```bash
mysql -u root -p
```
**Check devops db is created or not**
```bash
show databases;
```

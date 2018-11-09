## 5. Create a Deployment

Create `touch nginx-deployment.yaml`{{execute}}:

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxUnavailable: 2
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.15.4
        ports:
        - containerPort: 80
```{{copy}}

```kubectl apply -f nginx-deployment.yaml```

## 6. Create a Service

### 6.1. Create `touch service-clusterip.yaml`{{execute}}

```
apiVersion: v1
kind: Service
metadata:
  name: nginx
  labels:
    app: nginx
spec:
  type: ClusterIP
  ports:
  - port: 80
    protocol: TCP
  selector:
    app: nginx
```{{copy}}

```kubectl apply -f service-clusterip.yaml```{{execute}}
```kubectl get services```{{execute}} ```kubectl get svc```{{execute}}

Ping the ClusterIP from node01
```ssh node01```{{{execute}}}

On a worker node do
```
$ kubectl exec -it nginx -- bash
 $ ping nginx
 $ exit
```
### 6.2. Create a NodePort service
```
apiVersion: v1
kind: Service
metadata:
  name: nginx
  labels:
    app: nginx
spec:
  type: NodePort
  ports:
  - port: 80
    nodePort: 30080
    protocol: TCP
  selector:
    app: nginx
```
On a manager node, create a `service-nodeport.yaml` file with that service definition and do:
```
$ kubectl apply -f service-nodeport.yaml

# Notice the service type has been updated
$ kubectl get services

# Get the Kubernetes node's IPs
$ kubectl get node -o wide
```


Use the internal ip and do:
```
$ curl ${internalIp}:30080
```

## 7. Create an Ingress

### 7.1. Create Services
Create `nginx-deployment.yaml`
```
kind: Service
apiVersion: v1
metadata:
  name: nginx
spec:
  selector:
    app: nginx
  ports:
  - protocol: TCP
    port: 80
    targetPort: 80
---
kind: Deployment
apiVersion: apps/v1beta1
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.7.9
        ports:
        - containerPort: 80
```
```
$ kubectl apply -f nginx-deployment.yaml
```

Create `apache-deployment.yaml`
```
kind: Service
apiVersion: v1
metadata:
  name: httpd
spec:
  selector:
    app: httpd
  ports:
  - protocol: TCP
    port: 80
    targetPort: 80
---
kind: Deployment
apiVersion: apps/v1beta1
metadata:
  name: httpd-deployment
  labels:
    app: httpd
spec:
  replicas: 3
  selector:
    matchLabels:
      app: httpd
  template:
    metadata:
      labels:
        app: httpd
    spec:
      containers:
      - name: httpd
        image: httpd:2.4.33
        ports:
        - containerPort: 80
```

```
$ kubectl apply -f apache-deployment.yaml
```

### 7.2. Create the Ingress
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: test
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
  - host: ingress.test.com
    http:
      paths:
      - path: /nginx
        backend:
          serviceName: nginx
          servicePort: 80
      - path: /apache
        backend:
          serviceName: httpd
          servicePort: 80
```

### 7.3. Test the Ingress
Add the ingress to your hosts
```
$ printf 127.0.0.1 ingress.test.com | sudo tee -a /etc/hosts
```
Test the ingress:
```
$ curl ingress.test.com
default backend - 404
```
Test nginx:
```
$ curl ingress.test.com/nginx
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
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
```
Test apache:
```
$ curl ingress.test.com/apache
```

## 8. Persistent Volumes and Claims
### 8.1. Make a Mount Path
```
$ mkdir /mnt/data
$ echo 'Hello from Kubernetes storage' > /mnt/data/index.html
```
### 8.2. HostPath Volume
Create `persistent-volume.yaml`:
```
kind: PersistentVolume
apiVersion: v1
metadata:
  name: task-pv-volume
  labels:
    type: local
spec:
  storageClassName: manual
  capacity:
    storage: 10Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: "/mnt/data"
```
```
$ kubectl apply -f persistent-volume.yaml
```
```
$ kubectl get pv task-pv-volume
NAME             CAPACITY   ACCESSMODES   RECLAIMPOLICY   STATUS      CLAIM     STORAGECLASS   REASON    AGE
task-pv-volume   10Gi       RWO           Retain          Available             manual                   4s
```

### 8.3. Persistent Volume Claim
Create `persistent-volume-claim.yaml`:
```
kind: PersistentVolumeClaim
apiVersion: v1
metadata:
  name: task-pv-claim
spec:
  storageClassName: manual
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 3Gi
```
```
$ kubectl apply -f persistent-volume-claim.yaml
```
```
$ kubectl get pv task-pv-volume
NAME             CAPACITY   ACCESSMODES   RECLAIMPOLICY   STATUS    CLAIM                   STORAGECLASS   REASON    AGE
task-pv-volume   10Gi       RWO           Retain          Bound     default/task-pv-claim   manual                   2m
```
```
$ kubectl get pvc task-pv-claim
NAME            STATUS    VOLUME           CAPACITY   ACCESSMODES   STORAGECLASS   AGE
task-pv-claim   Bound     task-pv-volume   10Gi       RWO           manual         30s
```
### 8.4. Create a Pod
```
kind: Pod
apiVersion: v1
metadata:
  name: task-pv-pod
spec:
  volumes:
    - name: task-pv-storage
      persistentVolumeClaim:
       claimName: task-pv-claim
  containers:
    - name: task-pv-container
      image: nginx:1.7.9
      ports:
        - containerPort: 80
          name: "http-server"
      volumeMounts:
        - mountPath: "/usr/share/nginx/html"
          name: task-pv-storage
```
### 8.5. Check what nginx is serving
Exec into the pod and curl localhost
```
$ kubectl exec -it task-pv-pod -- /bin/bash
  $ apt-get update
  $ apt-get install curl
  $ curl localhost
Hello from Kubernetes storage
```
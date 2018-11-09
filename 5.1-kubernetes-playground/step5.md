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

### 6.1. ClusterIP

Create `touch service-clusterip.yaml`{{execute}} with contents:
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
```kubectl get services```{{execute}}
```kubectl get svc```{{execute}}

WRTITE DOWN THE CLUSTER-IP

Ping the ClusterIP from node01
```ssh node01```{{{execute}}}

Cluster IP (VIP) is routable from any of the pods
```
kubectl exec -it nginx -- bash
apt install curl >> /dev/null
curl <<CLUSTER-IP>>
exit
```

### 6.2. Create a NodePort service

Create a `touch service-nodeport.yaml`{{execute}} with contents:
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
```{{copy}}

```kubectl apply -f service-nodeport.yaml```{{execute}}

# Notice the service type has been updated
```kubectl get services```{{execute}}

# Get the Kubernetes node's IPs
```kubectl get node -o wide```{{execute}}

Use the internal ip and execute:
```curl node01:30080```{{execute}}

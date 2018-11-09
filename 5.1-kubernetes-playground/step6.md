## 7. Create an Ingress

### 7.1. Create Services
Create a `touch nginx-deployment.yaml`{{execute}} with contents:
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
```{{copy}}

```kubectl apply -f nginx-deployment.yaml```{{execute}}

Create `touch apache-deployment.yaml`{{execute}} with contents:
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
```{{copy}}

```kubectl apply -f apache-deployment.yaml```{{execute}}

### 7.2. Create the Ingress
Create `touch test-ingress.yaml`{{execute}} with contents:
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
```{{copy}}

```kubectl apply -f test-ingress.yaml```


### 7.3. Test the Ingress

Test nginx:
```curl -H "Host: ingress.test.com" <<IP>>/nginx```{{copy}}

Test apache:
```curl -H "Host: ingress.test.com" <<HOST>>/apache```{{copy}}

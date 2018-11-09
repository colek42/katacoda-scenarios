### 3.1. Nginx Pod

Create `nginx-pod.yaml`:

```
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx:1.7.9
    ports:
    - containerPort: 80
```

```kubectl apply -f nginx-pod.yaml```{{execute}}

#### Viewing the Pods
```kubectl get pods``{{execute}}

```kubectl get pods --show-labels```{{execute}}

```kubectl get pods -o wide```{{execute}}

```kubectl get pods nginx -o yaml```{{execute}}

#### Pod Details
```kubectl describe pods nginx```{{execute}}

#### Delete the Pod
Run either
```kubectl delete -f nginx-pod.yaml```{{execute}}

```kubectl delete pods nginx```{{execute}}

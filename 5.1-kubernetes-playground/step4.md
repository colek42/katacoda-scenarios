## 4. ConfigMaps and Secrets
### 4.1. Create a ConfigMap
Make `touch api-configmap.yaml`{{execute}}
```
apiVersion: v1
kind: ConfigMap
metadata:
  name: api-config
data:
  endpoint: api.boxboat.net
  port: "80"
```{{copy}}

```kubectl apply -f api-configmap.yaml```{{execute}}

### 4.2. Create a secret
Create base64 encoded values for a username/password

```printf 'admin' | base64```{{execute}}

```printf 'password' | base64```{{execute}}


Create `touch api-secrets.yaml`{{execute}}:
```
apiVersion: v1
kind: Secret
metadata:
  name: api-secrets
type: Opaque
data:
  username: YWRtaW4=
  password: cGFzc3dvcmQ=
```{{copy}}

```kubectl apply -f api-secrets.yaml```{{execute}}

### 4.3. Use the ConfigMap and Secret
Create `touch pod-config.yaml`{{execute}}:
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
    env:
    - name: API_USER
      valueFrom:
        secretKeyRef:
          key: username
          name: api-secrets
    - name: API_PASS
      valueFrom:
        secretKeyRef:
          key: password
          name: api-secrets
    - name: API_ENDPOINT
      valueFrom:
        configMapKeyRef:
          key: endpoint
          name: api-config
    - name: API_PORT
      valueFrom:
        configMapKeyRef:
          key: port
          name: api-config
```{{copy}}

Describe the pod  
```kubectl describe pod nginx```{{execute}}

Read the data from the container:
```kubectl exec -it nginx -- env | grep API```{{execute}}
## 8. Persistent Volumes and Claims
### 8.1. Make a Mount Path

```mkdir /mnt/data && cd ~```{{execute}}  
```echo 'Hello from Kubernetes storage' > /mnt/data/index.html```{{execute}}  


### 8.2. HostPath Volume
Create `touch persistent-volume.yaml`{{execute}} with the following content:
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
```{{copy}}

```kubectl apply -f persistent-volume.yaml```{{execute}}

```kubectl get pv task-pv-volume```{{execute}}

### 8.3. Persistent Volume Claim
Create `touch persistent-volume-claim.yaml`{{execute}} with the following content:
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
```{{copy}}
```kubectl apply -f persistent-volume-claim.yaml```{{execute}}
```kubectl get pv task-pv-volume```{{execute}}
```kubectl get pvc task-pv-claim```{{execute}}

### 8.4. Create a Pod

Create `touch data-pod.yaml`{{execute}} with the following content:
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
```{{copy}}

```kubectl apply -f data-pod.yaml```{{execute}}


### 8.5. Check what nginx is serving
Exec into the pod and curl localhost
```kubectl exec -it task-pv-pod -- /bin/bash```{{execute}}  
```apt-get update && apt-get -y install curl```{{execute}}  
```curl localhost```{{execute}}  

- Try changing the content in the mount.  What happens when you curl the pod again?
- Persistence is complex.  https://kubernetes.io/docs/concepts/storage/persistent-volumes

- Survey https://docs.google.com/forms/d/1lYZXTY5RIbcE2GFrKnCsA726nCjpaQ0DSkTYFImo05A
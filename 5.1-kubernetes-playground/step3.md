### 3.2. Multi-Container Pod
Make a file `touch two-container-pod.yaml`{{execute}}:
```
apiVersion: v1
kind: Pod
metadata:
  name: two-containers
spec:
  restartPolicy: Never
  volumes:
  - name: shared-data
    emptyDir: {}

  containers:
  - name: nginx-container
    image: nginx:1.7.9
    volumeMounts:
    - name: shared-data
      mountPath: /usr/share/nginx/html

  - name: debian-container
    image: debian
    volumeMounts:
    - name: shared-data
      mountPath: /pod-data
    command: ["/bin/sh"]
    args: ["-c", "echo Hello from the debian container > /pod-data/index.html"]
```{{copy}}

```kubectl apply -f two-container-pod.yaml```{{execute}}
#### Show the Pod
```kubectl get pods --watch```{{execute}}

Why is there only 1 container of 2?
Look at the debain container, it writes a file and exits. And the `restartPolicy` is set to `Never`

#### Check the file contents
```kubectl -it exec two-containers -c nginx-container cat /usr/share/nginx/html/index.html```{{execute}}

#### Delete the pod
```kubectl delete -f two-container-pod.yaml```{{execute}}

### 3.3. Pod with Environmental Variables
Create `touch pod-with-env.yaml`{{execute}}:
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
    - name: ENV_VAR
      value: ENV_VAL
```{{copy}}

```kubectl apply -f pod-with-env.yaml```{{execute}}

#### Check the Environment
Describe the pod. Notice the "Environment" section:
```kubectl describe pod nginx```{{execute}}

Check your local env variables:
```env | grep ENV```{{execute}}

Check the contaienrs env vars:
```kubectl exec -it nginx -- env | grep ENV```{{execute}}


#### Delete the pod
```kubectl delete -f pod-with-env.yaml```{{execute}}
- Launch a Centos container called "centos1" in detached mode:
```docker container run -d -it --name centos1 centos```{{execute}}

- Inspect the bridge network and note the container IP address:
```docker network inspect bridge```{{execute}}
If you look at the "Containers" filed you will find the IPv4 address.  
Store this value in an environment variable.
```export CENTOS_IP=$(docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' $(docker ps -q))```{{execute}}

- Launch another Centos container called "centos2" and get terminal access:
```docker container run -it --name centos2 centos bash```

- Ping your "centos1" container using the IP address from Step 3.
```ping ${CENTOS_IP}```{{execute}}
`CTRL-C` to exit the `centos2`

- Try and ping "centos1" using the container name:
```ping centos1```{{execute}}

- Why can we ping the container from inside the centos2 container, but not from the client?
- Launch a Centos container called "centos1" in detached mode:
```docker container run -d - it --name centos1 centos```{{execute}}

- Inspect the bridge network and note the container IP address:
```docker network inspect bridge```{{execute}}
If you look at the "Containers" filed you will find the IPv4 address.

- Launch another Centos container called "centos2" and get terminal access:
```docker container run -it --name centos2 centos bash```

5. Ping your "centos1" container using the IP address from Step 3.
```ping <ip address>```{{execute}}

6. Try and ping "centos1" using the container name:
```ping centos1```{{execute}}
It doesn't work!
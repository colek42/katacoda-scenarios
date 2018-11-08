- Create a new bridge network called "my_bridge":
```docker network create --driver bridge my_bridge```{{execute}}

- Verify and inspect your new network:
```docker network ls```{{execute}}
```docker network inspect my_bridge```{{execute}}

- Launch a Centos container in the background on your "my_bridge" network named "centos3":
```docker container run -d -it --network my_bridge --name centos3 centos```{{execute}}

- Launch another Centos container on your "my_bridge" network and connect to it:
```docker container run -it --network my_bridge --name centos4 centos```{{execute}}

- Open the `/etc/hosts` file:
```cat /etc/hosts```{{execute}}
Do you see centos3? Why didn't we in the previous exercise?

- Ping "centos3" using the container name:
```ping centos3```{{execute}}

- Ping "centos1" and "centos2" using the container name:
```ping centos1```{{execute}}
```ping centos2```{{execute}}
- View available Docker Volumes on your host:  
```docker volume ls```{{execute}} 

- Create a new Named volume:  
```docker volume create myvolume```{{execute}}

- Inspect your new volume:  
```docker volume inspect myvolume```{{execute}}
Take note the `"Mountpoint"`.

- Run a new container with your Named volume mounted:  
```docker container run --rm -it -v myvolume:/myloc/tmp --name mycontainer alpine```{{execute}}

- Create a temporary file in the container and exit:  
```touch /myloc/tmp/test.txt```{{execute}}
```exit```{{execute}}

- 
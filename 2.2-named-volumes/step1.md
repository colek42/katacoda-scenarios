- View available Docker Volumes on your host:  
```docker volume ls```{{execute}} 

- Create a new Named volume:  
```docker volume create myvolume```{{execute}}

- Inspect your new volume:  
```docker volume inspect myvolume```{{execute}}

- Store the mountpoint in a variable:
```export MOUNTPOINT=$(docker volume inspect myvolume | jq -r .[0].Mountpoint)```{{execute}}

- Run a new container with your Named volume mounted:  
```docker container run --rm -it -v myvolume:/myloc/tmp --name mycontainer alpine```{{execute}}

- Create a temporary file in the container and exit:  
```touch /myloc/tmp/test.txt```{{execute}}
```exit```{{execute}}

- View the contents of Volume "myvolume" on the remote host.  Notice we are mounting directly from the filesystem.
```docker container run --rm -v "${MOUNTPOINT}:/other/location" --name othercontainer alpine ls /other/location```{{execute}}

- Why can we not mount directly from the client?  Hint `echo $DOCKER_HOST`{{execute}}
- Why would you want to use a volume rather than a host mount?
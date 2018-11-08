In this lab, we will learn how to use host mounts as a volume.

- Run a container with attached host volume, since docker is running on a remote host we will mount a volume to the container and create a file.  If running locally you can mount a local directory in the same manner.
```docker container run -d -v ~/myloc:/myloc --name mycontainer1 ubuntu bash -c "touch /myloc/hello.txt"```{{execute}}
The `-v` flag is for volume. What are the other new flags?

- The container is still stopped
```docker container ls -a```{{execute}}

- View the mounted file in the container and exit:
```docker container run --rm -it -v ~/myloc:/myloc --name mycontainer2 alpine ls /myloc```{{execute}}
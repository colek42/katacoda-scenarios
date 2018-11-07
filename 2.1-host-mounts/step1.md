In this lab, we will learn how to use host mounts as a volume.

- Run a container with attached host volume, since docker is running on a remote host we will mount a volume to the container and create a file

```docker container run -d -v ~/myloc:/myloc --name mycontainer1 ubuntu bash -c "touch /myloc/hello.txt && sleep 1000"```{{execute}}
The `-v` flag is for volume. What are the other new flags?

- The container is still running
```docker container ls -a```{{execute}}

- View the mounted file in the container and exit:
```docker container run --rm -it -v ~/myloc:/myloc --name mycontainer2 alpine cat hello.txt```{{execute}}
```exit```{{execute}}


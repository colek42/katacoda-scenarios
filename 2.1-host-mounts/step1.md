In this lab, we will learn how to use host mounts as a volume.

- Run a container with attached host volume, since docker is running on a remote host we will mount a volume to the container and create a file

```docker container run --rm -it -v ~/myloc:/myloc --name mycontainer alpine touch hello.txt```{{execute}}
The `-v` flag is for volume. What are the other new flags?

- With the `--rm` flag the container gets removed on exit.  Check to make sure container has been removed.
```docker container ls -a```{{execute}}

- View the mounted file in the container and exit:
```docker container run --rm -it -v ~/myloc:/myloc --name mycontainer alpine cat hello.txt```{{execute}}
```exit```{{execute}}


In this lab, we will learn how to use host mounts as a volume.

- Run a container with attached host volume, there is a file named `hello.txt` located on the host server.

```docker container run --rm -it -v ~/myloc:/myloc --name mycontainer alpine```{{execute}}
The `-v` flag is for volume. What are the other new flags?

- View the mounted file in the container and exit:
```cat /myloc/hello.txt```{{execute}}
```exit```{{execute}}

- View the contents of `test.txt` on the host filesystem:
```ls -al ~/mytmp```{{execute}}

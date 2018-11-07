In this lab, we will learn how to use host mounts as a volume.

- Run a container with attached host volume:


```mkdir -p /mytmp```{{execute}}
```docker container run --rm -it -v /mytmp:/myloc/tmp --name mycontainer alpine```{{execute}}
The `-v` flag is for volume. What are the other new flags?

- Create a temporary file in the container and exit:
```echo "Hello Docker" > /myloc/tmp/test.txt```{{execute}}
```exit```{{execute}}

- View the contents of `test.txt` on the host filesystem:
```ls -al /tmp```{{execute}}

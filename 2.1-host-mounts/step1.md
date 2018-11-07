In this lab, we will learn how to use host mounts as a volume.

- Run a container with attached host volume:
```
docker container run --rm -it -v /tmp:/myloc/tmp --name mycontainer alpine
```{{execute}}
The `-v` flag is for volume. What are the other new flags?

- Create a temporary file in the container and exit:
```
touch /myloc/tmp/test.txt
exit
```{{execute}}

- View the contents of `test.txt` on the host filesystem:
```
ls /tmp
```{{execute}}

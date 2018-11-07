1. Create a container using the Ubuntu 16.04 image and connect to STDIN and a terminal:

`docker run -it ubuntu:16.04 bash`{{execute}}

This will run the container, attach to standard input stream, and get a pseudo-terminal. For the container process, we specify bash to get the terminal.

2. Create a file using the touch command:

`touch test && ls`{{execute}}

You should see the file created in the root directory of the container. Now exit:
```
exit
```{{execute}}

3. Run the container once again:
```
docker run -it ubuntu:16.04 bash && \
ls
```{{execute}}
Where did our file go?
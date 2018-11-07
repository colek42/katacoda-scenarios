- Run Centos image in detached mode:  
`docker container run -d centos ping 8.8.8.8 -c 50`{{execute}}

2. Attach to the newly created container and observe the output:

```
docker container attach <container id>
```
	
3. Try to detach from the container by entering CTRL+P+Q at once. 
Nothing happens...exit using CTRL+C.

4. Check your running containers and notice that your container is no longer running:

```
docker container ls
```
The CTRL+C combination killed the container process.

5. Run another container, but this time with `-it` passed:

```
docker container run -d -it centos ping 8.8.8.8 -c 50
```

6. Attach and detach from container:

```
docker container attach <container id>
```
CTRL+P+Q

7. Show your container still running in the background:

```
docker container ls
```
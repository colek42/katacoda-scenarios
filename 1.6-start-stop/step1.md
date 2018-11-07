1. Run Tomcat:7 image in detached mode:

```
docker container run -d tomcat:7 --name=tomcat
```{{execute}}

2. Follow the logs of the newly created container and observe the output:

```
docker container logs -f $(docker ps -q)
```{{execute}}
You should see output of Tomcat server starting up.
(CTRL+C)
	
3. Stop the container

```
docker container ls
docker container stop $(docker ps -q)
```{{execute}}

4. Start the container and attach to stdout:

```
docker container start -a $(docker ps -a -q)
```{{execute}}
We're back up and running! You will see Tomcat server go through its startup routine again. 
Bonus: We saved our filesystem, but not memory and running processes. Reasearch `docker checkpoint`

5. Detach and exit from container using CTRL+C

6. Start container again, but without `-a` (attach):

```
docker container start $(docker ps -a -q)
```{{execute}}
Your container is now running again, but this time in the background.
```
docker container ls
```{{execute}}
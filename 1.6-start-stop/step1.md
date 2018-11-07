- Run Tomcat:7 image in detached mode:
```
docker container run -p 8080:8080 -d tomcat:7
```{{execute}}

(The Tomcat welcome page can be viewed here https://[[HOST_SUBDOMAIN]]-80-[[KATACODA_HOST]].environments.katacoda.com/ )

- Follow the logs of the newly created container and observe the output:
```
docker container logs -f $(docker ps -q)
```{{execute}}
You should see output of Tomcat server starting up.
(CTRL+C)
	
- Stop the container:  
`docker container ls`{{execute}}  
`docker container stop $(docker ps -q)`{{execute}}

- Start the container and attach to stdout:
```
docker container start -a $(docker ps -a -q)
```{{execute}}
We're back up and running! You will see Tomcat server go through its startup routine again. 
Bonus: We saved our filesystem, but not memory and running processes. Reasearch `docker checkpoint`

Detach and exit from container using CTRL+C

- Start container again, but without `-a` (attach):
```
docker container start $(docker ps -a -q)
```{{execute}}
Your container is now running again, but this time in the background.
```
docker container ls
```{{execute}}
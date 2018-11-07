In this lab, we will learn how to interact with an already running Docker container.

- Run the NGINX image (a web server that services http requests external to the host) and connect to STDIN and a terminal:  
`docker container ls`{{execute}}  
`docker container run -it --rm --name nginx nginx`{{execute}}  
ctrl+c to exit.

- Run the NGINX image in detached mode:  
`docker container ls`{{execute}}  
`docker container run -p 80:80 -d --rm --name nginx nginx`{{execute}}  
`docker container ls`{{execute}}  

Notice the port mapping - port 80 has been mapped to a random port on the host. 
Go to server https://[[HOST_SUBDOMAIN]]-80-[[KATACODA_HOST]].environments.katacoda.com/ and see the NGINX welcome page.

- Gain terminal access to detached container:  
`docker container exec -it nginx bash`{{execute}}  
`exit`{{execute}}  
`docker container ls`{{execute}}  
`docker rm $(docker kill nginx)`{{execute}}  
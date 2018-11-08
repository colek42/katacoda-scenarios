- Run NGINX without any port mapping:  
```docker container run -d --name nginx-exposed nginx```{{execute}}

- Show nginx exposed port 80:
```docker container ls -a```{{execute}}
Visit https://[[HOST_SUBDOMAIN]]-80-[[KATACODA_HOST]].environments.katacoda.com/ why isn't it working?

- Run an nginx container and map port 80 on the container to port 8080 on your host. Map port 443 on the container to port 4443 on the host:
```docker container run -d -p 8080:80 -p 4443:443 nginx```{{execute}}
Visit https://[[HOST_SUBDOMAIN]]-8080-[[KATACODA_HOST]].environments.katacoda.com/ and check if it is working!  
Visit https://[[HOST_SUBDOMAIN]]-4443-[[KATACODA_HOST]].environments.katacoda.com/ and check if it is working!  

- Verify open ports:
```docker container ls```{{execute}}
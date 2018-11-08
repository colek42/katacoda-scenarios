- Publishing Ports  
Run Nginx and let Docker choose a high port:  
```docker container run -d -p 80 --name nginx-high nginx```{{execute}}
Visit https://[[HOST_SUBDOMAIN]]-<<HIGH PORT>>-[[KATACODA_HOST]].environments.katacoda.com/

- Run Nginx and let Docker determine exposed ports and mapping: 
```docker container run -d -P --name nginx-auto nginx```{{execute}}
Visit https://[[HOST_SUBDOMAIN]]-<<HIGH PORT>>-[[KATACODA_HOST]].environments.katacoda.com/

- Show open ports:  
```docker container ls```{{execute}}

- For communication within the network, use the exposed port.
- For communication external to the network, use the published port.  
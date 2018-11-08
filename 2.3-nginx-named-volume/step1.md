In this lab, we will run Nginx with the content of our local host directory.

- Create a new directory "content" with file "index.html" on your host. Add some text to the file.
```docker container run -d -v ~/myloc:/myloc --name mycontainer3 ubuntu bash -c "echo 'Hello World' > /myloc/index.html"```{{execute}}

- Run NGINX container using a host volume mount
```docker container run -p 80:80 -v ~/myloc:/usr/share/nginx/html nginx```{{execute}}
Visit https://[[HOST_SUBDOMAIN]]-80-[[KATACODA_HOST]].environments.katacoda.com/ and check if it is working!  
`CTRL-C to exit`

Did it work? What happened to the content that was already at the path /usr/share/nginx/html?

Can you change the content without stopping the NGINX service?
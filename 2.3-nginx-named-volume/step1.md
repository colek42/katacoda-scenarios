In this lab, we will run Nginx with the content of our local host directory.

- Create a new directory "content" with file "index.html" on your host. Add some text to the file.
```docker container run -v /root/www/html:/mnt/html alpine echo "Hello World" > index.html```{{execute}}

- Run NGINX container using a host volume mount
```docker container run -p 80:80 -d -v /root/www/html:/usr/share/nginx/html nginx```{{execute}}

https://[[HOST_SUBDOMAIN]]-80-[[KATACODA_HOST]].environments.katacoda.com/

Did it work? What happened to the content that was already at the path /usr/share/nginx/html?
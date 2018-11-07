- Run Centos image in detached mode:  
`docker container run -d centos ping 8.8.8.8 -c 50`{{execute}}

- Attach to the newly created container and observe the output:  
`docker container attach $(docker container ls -q)`{{execute}}  
exit using CTRL+C.

- Check your running containers and notice that your container is no longer running:  
`docker container ls`{{execute}}  
The CTRL+C combination killed the container process.

- Run another container, but this time with `-it` passed:  
`docker container run -d -it centos ping 8.8.8.8 -c 50`{{execute}}

- Attach and detach from container:  
`docker container attach $(docker container ls -q)`  
`^P^Q` NOTE:  This will not work in katacoda

7. Show your container still running in the background:  
`docker container ls`
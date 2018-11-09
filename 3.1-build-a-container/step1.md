- Pull the boxboat repository
```git clone https://github.com/boxboat/bootcamp-labs.git```{{execute}}

- Navigate to the correct directory
```cd /home/scrapbook/tutorial/bootcamp-labs/simple-python-app/```{{execute}}

- Build the image
```docker build –t python-app:latest .```{{execute}}

- Where is the image?
```docker image ls```{{execute}}

- Tag the image again.
```docker image tag python-app:latest python-app:v1.0-dev```{{execute}}





### Building a Custom Alpine Linux Image

To customize the Alpine Linux container, you can create a custom Docker image based on the Alpine Linux base image. Here's an example Dockerfile:


Docker apline Container with nginx
Dockerfile

```
FROM alpine:latest 

## Install additional packages 
RUN apk add --no-cache nginx 

## Copy custom configuration files 
COPY nginx.conf /etc/nginx/nginx.conf 

## Expose the necessary port 
EXPOSE 80 

## Set the default command 
CMD ["nginx", "-g", "daemon off;"]

```


In this Dockerfile, we:

1. Start with the latest Alpine Linux base image.
2. Install the Nginx web server using the `apk` package manager.
3. Copy a custom Nginx configuration file to the container.
4. Expose port 80 for the Nginx web server.
5. Set the default command to start the Nginx web server.


To build the custom image, run the following command:
Now lets build image from dockerfile using below command
```
docker build -t my-alpine-nginx .
```


This will create a new Docker image named `my-alpine-nginx` based on the Dockerfile in the current directory.



### Running the Custom Alpine Linux Container

Once you have the custom image, you can run a container based on it:

```
docker run -d -p 8080:80 my-alpine-nginx
```

In this command, we:

- `-d`: Run the container in detached mode (in the background).
- `-p 8080:80`: Map port 8080 on the host to port 80 in the container.
- `my-alpine-nginx`: The name of the custom Alpine Linux image we created.

Now, you can access the Nginx web server running in the container by visiting `http://localhost:8080` in your web browser.



```
docker run -it --rm alpine /bin/ash
```
- `/bin/ash` is Ash ([Almquist Shell](http://www.in-ulm.de/~mascheck/various/ash/#busybox)) provided by BusyBox
- `--rm` Automatically remove the container when it exits (`docker run --help`)
- `-i` Interactive mode (Keep STDIN open even if not attached)
- `-t` Allocate a pseudo-TTY


Check current version of alpine os
`cat /etc/alpine-release`





```
kernel version
uname -a

current user
whoami

free and used memory
free

current working directory
pwd

```




## Pull the Alpine Linux image 

docker pull alpine:latest 
## Run an Alpine Linux container 

docker run -it alpine:latest /bin/ash


uname -a 
apk add --no-cache htop / 
htop


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




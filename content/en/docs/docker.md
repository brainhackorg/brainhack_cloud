---
title: "Installing and Using Docker"
linkTitle: "Docker"
weight: 100
description: >-
     Installing and using Docker 
---


## Overview
Docker (or a software container in general) is great for reproducibility and making it easy to move your tools in and out of the cloud. If you don't know what containers are, here is a 3 minute explanation: https://www.youtube.com/watch?v=HelrQnm3v4g

You can either install the original "Docker" or a drop-in replacement called "Podman"

## Installing Docker
Docker is not installed by default on Oracle Linux and these steps will install and start Docker:

```
sudo dnf install dnf-utils zip unzip
sudo dnf config-manager --add-repo=https://download.docker.com/linux/centos/docker-ce.repo
sudo dnf install docker-ce --nobest
sudo systemctl enable docker.service
sudo systemctl start docker.service
```

## OR: Installing Podman instead of Docker
Podman is compatible with docker and is the default in Oracle Linux (and some argue it's even better). This is how to install podman:

```
sudo yum install docker
```
Yes: This is actually installing podman in Oracle Linux!

Or the direct way:
```
sudo yum install podman
```

If you installed podman using `sudo yum install docker` you can run docker commands directly, but it will tell you that this is actually podman:
```
docker
```
![image](https://user-images.githubusercontent.com/4021595/160227312-43836181-cd0c-4d26-bc18-9ccc9fb0f700.png)

Let's remove that msg:
```
sudo touch /etc/containers/nodocker
```

Now we have podman installed as "docker" drop-in replacement and we can test it:
```
docker run hello-world
```
![image](https://user-images.githubusercontent.com/4021595/160227372-0012e396-a2ca-4f80-8455-d4b2c8616b19.png)


and we could now run everything we like, e.g. https://neurodesk.github.io/docs/neurodesktop/getting-started/linux/
```
mkdir /home/opc/neurodesktop-storage
sudo docker run \
  --shm-size=1gb -it --privileged --name neurodesktop \
  -v ~/neurodesktop-storage:/neurodesktop-storage \
  -e HOST_UID="$(id -u)" -e HOST_GID="$(id -g)"\
  -p 8080:8080 \
  -h neurodesktop-20220302 docker.io/vnmd/neurodesktop:20220329
 ```
 
 and this is how easy it is to run a container on the cloud :)
 
 if you connect to your cloud instance using a port-forwarding `ssh -L 8080:127.0.0.1:8080 opc@xxx.xx.xx.xx` then you could now use Neurodesktop via visiting http://localhost:8080/#/?username=user&password=password in your local browser. When done, stop the container with CTRL-C and run `sudo docker rm neurodesktop` to cleanup.

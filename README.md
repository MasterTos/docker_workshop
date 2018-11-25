# Docker and Compose 101
Docker and Compose 101 workshop - Introductions to Docker and Compose.

## Prerequisites
* Install [Docker toolbox](https://docs.docker.com/toolbox/overview/#whats-in-the-box) for Windows/Mac
* For Linux follow instructions [here](https://docs.docker.com/install/).

## Outline
- [Docker and Compose 101](#docker-and-compose-101)
  - [Prerequisites](#prerequisites)
  - [Outline](#outline)
  - [How to use Docker Machine](#how-to-use-docker-machine)
  - [Introduction](#introduction)
    - [Hello, world](#hello-world)
    - [Envrionment variables](#envrionment-variables)
    - [Host mounts volume](#host-mounts-volume)
    - [Publish container port](#publish-container-port)
  - [Basic container operations](#basic-container-operations)
    - [Interactive container](#interactive-container)
    - [Detach container](#detach-container)
    - [Attach container](#attach-container)
    - [Inspect and Log a running container](#inspect-and-log-a-running-container)
    - [Start stop and remove container](#start-stop-and-remove-container)
  - [Dockerfile](#dockerfile)
  - [Docker Compose](#docker-compose)

## How to use Docker Machine

After finished then run below command for check docker-machine
```bash
$ docker-machine --version #check version of docker-machine

$ docker-machine create --driver virtualbox -virtualbox-memory "1024" default # create new docker-machine

$ docker-machine ls # check ip address
```
*NOTE:* username/password is docker/tcuser

SSH to docker-machine
- `docker-machine ssh docker101` # default ssh via command prompt
- access via putty(windows) to ip address
- access via Shell (mac)

```bash
$ docker-machine stop default #to stop machine
$ docker-machine start default #to start machine
```

## Introduction
### Hello, world
```bash
$ docker container run busybox echo "Hello, World"
# Output:
Hello, World
```
### Envrionment variables
```bash
$ docker container run busybox env
# Output:
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
HOSTNAME=7c3779b93b52
HOME=/root
```
```bash
$ docker container run -e HELLO=WORLD busybox env
# Output:
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
HOSTNAME=b27d43baea8d
HELLO=WORLD
HOME=/root
```
### Host mounts volume
```bash
$ docker container run busybox ls -l /home
# Output:
total 0
```
We can use `-v` flag to mount directory to container:
```bash
$ docker container run -v $(pwd):/home busybox ls -l /home
# Output:
total 4
-rw-r--r--    1 1000     1000          2580 Nov 25 14:24 README.md
```

### Publish container port
We can use `-p` flag to forward a port on the host to the port 5000 inside the container:
```bash
$ docker container run -p 5000:5000 python:alpine python -m http.server 5000
```
*NOTE:* `-p <HostPort>:<ContainerPort>`

This command blocks because the server listens for requests, open a new tab and access the endpoint
```bash
$ curl <ip address>:5000
# Output:
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
<html>
<head>
....
```
Press `Ctrl-C` to stop the running container.

## Basic container operations
### Interactive container
```bash
$ docker container run -it busybox
# Output:
/ # ps
PID   USER     TIME  COMMAND
    1 root      0:00 sh
   10 root      0:00 ps
```
Press `Ctrl-C` to stop the running container.

### Detach container
```bash
$ docker container run -d -p 5000:5000 --name web python:alpine python -m http.server 5000
```
### Attach container
```bash
$ docker container exec -it web sh
```
We can look around to see the process running as PID 1:
```bash
/ # ps
PID   USER     TIME  COMMAND
    1 root      0:00 sh
   10 root      0:00 ps
# exit
```
- `-t` flag attaches terminal for interactive typing.
- `-i` flag attaches input/output from the terminal to the process.
### Inspect and Log a running container
We can use `ps` command to view all running containers:
```bash
$ docker container ps
# Output:
CONTAINER ID        IMAGE                   COMMAND                  CREATED             STATUS              PORTS                            NAMES
0d5f55ef5e85        python:alpine           "python -m http.serv…"   5 minutes ago       Up 5 minutes        0.0.0.0:5000->5000/tcp           webtest
```

We can check configuration of container with command:
```bash
$ docker container inspect webtest
```

We can check log of container with command:
```bash
$ docker container logs webtest
```

### Start stop and remove container
```bash
$ docker container start webtest
$ docker container stop webtest
$ docker container rm webtest
```

## Dockerfile

## Docker Compose

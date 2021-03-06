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
    - [Usage](#usage)
      - [Building](#building)
      - [Verisoning](#verisoning)
      - [.dockerignore file](#dockerignore-file)
    - [Dockerfile instructions](#dockerfile-instructions)
    - [Examples](#examples)
  - [Docker Compose](#docker-compose)
    - [Usage](#usage-1)
    - [Examples](#examples-1)

## How to use Docker Machine

After finished then run below command for check docker-machine
```bash
$ docker-machine --version #check version of docker-machine

$ docker-machine create --driver virtualbox -virtualbox-memory "1024" default # create new docker-machine

$ docker-machine ls # check ip address
```
*NOTE:* username/password is docker/tcuser

SSH to docker-machine
- `docker-machine ssh default` # default ssh via command prompt
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
/ # exit
```

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
/ # exit
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
Docker can build images automatically by reading the instructions from a `Dockerfile`. A `Dockerfile` is a text document that contains all the commands a user could call on the command line to assemble an image. Using `docker build` users can create an automated build that executes several command-line instructions in succession.

### Usage
#### Building
```bash
$ docker image build .
```
```bash
$ docker image build -f path/to/Dockerfile .
```
```bash
$ docker image build -t <user>/<appname> .
```
*NOTE:* Default tag is `:latest`
#### Verisoning
```bash
$ docker image build -t <user>/<appname>:<tag> .
```
```bash
$ docker image build -t <user>/<appname>:<tag> -t <user>/<appname>:latest .
```

#### .dockerignore file
Before the docker CLI sends the context to the docker daemon, it looks for a file named `.dockerignore` in the root directory of the context. If this file exists, the CLI modifies the context to exclude files and directories that match patterns in it. 

Here is an example `.dockerignore` file:
```
# comment
*/temp*
*/*/temp*
temp?
```
### Dockerfile instructions
- `FROM` : The `FROM` instruction initializes a new build stage and sets the Base Image for subsequent instructions.
```yaml
FROM <image>[:<tag>] [AS <name>]
```
- `LABEL` : The `LABEL` instruction adds metadata to an image.
```yaml
LABEL <key>=<value>
LABEL <key>=<value>
LABEL ...
```

- `RUN` : The `RUN` instruction will execute any commands in a new layer on top of the current image and commit the results. The resulting committed image will be used for the next step in the `Dockerfile`.
```yaml
RUN <command>
```

- `CMD` : The main purpose of a `CMD` is to provide defaults for an executing container.
```yaml
CMD [“executable”, “param1”, “param2”…]
```

- `EXPOSE` : The `EXPOSE` instruction informs Docker that the container listens on the specified network ports at runtime.
```yaml
EXPOSE <port> [<port>/<protocol>...]
```

- `ENV` : The `ENV` instruction sets the environment variable `<key>` to the value `<value>`.
```yaml
ENV <key> <value>
ENV <key>=<value> ...
``` 
  
- `ADD` or `COPY` : The `ADD` instruction copies new files, directories or remote file URLs from `<src>` and adds them to the filesystem of the image at the path `<dest>`.
```yaml
ADD [--chown=<user>:<group>] <src>... <dest>
```
*NOTE:* `COPY` is same as `ADD`, but without the tar and remote URL handling.

- `ENTRYPOINT` : An `ENTRYPOINT` allows you to configure a container that will run as an executable.
```yaml
ENTRYPOINT ["executable", "param1", "param2"]
```


- `VOLUME` : The `VOLUME` instruction creates a mount point with the specified name and marks it as holding externally mounted volumes from native host or other containers. 
```yaml
VOLUME ["/data"]
```

- `WORKDIR` : The `WORKDIR` instruction sets the working directory for any `RUN`, `CMD`, `ENTRYPOINT`, `COPY` and `ADD` instructions that follow it in the Dockerfile.
```yaml
WORKDIR /path/to/workdir
```

- etc.
### Examples
- Entrypoint
```yaml
# docker/entrypoint/Dockerfile
FROM busybox
ADD hello.sh /hello.sh
ENTRYPOINT ["/hello.sh"]
```
```bash
# docker/entrypoint/hello.sh

#!/bin/sh

echo "hello, world $@!"
```
```bash
$ cd docker/entrypoint
$ docker image build -t hello .
$ docker container run hello
$ docker container run hello EI-workshop
```
- Django
```yaml
# docker/django/Dockerfile
FROM mastertos/django:base
WORKDIR /code
ADD example /code
EXPOSE 8000
ENTRYPOINT ["python", "manage.py", "runserver", "0.0.0.0:8000"]
```
```bash
$ cd docker/django
$ docker image build -t django:example .
$ docker container run --rm django:example
```

## Docker Compose
### Usage
```bash
docker-compose up
docker-compose down
docker-compose exec <name> <cmd>
docker-compose -f /path/to/docker-compose/file [up,down,...]
```
### Examples
- Single Container
```yaml
# docker/compose/docker-compose.yml
version: '3'

services: # these are all the services that a docker app uses
  django-web: # this is the name of the service we're creating; it's chosen by us. Here, we're calling it "django-web".
    image: django-web # this is the name of the image to us
    container_name: django-web # this is the name of the container to us
    build:
      context: ../django
      dockerfile: Dockerfile
    ports:
      - 8000:8000
    volumes:
      - ../django/example:/code
```
```bash
$ cd docker/compose
$ docker-compose up
```
- Multiple Containers
```yaml
version: '3'

services: # these are all the services that a docker app uses
  django-web2: # this is the name of the service we're creating; it's chosen by us. Here, we're calling it "django-web2".
    image: django-web2 # this is the name of the image to us
    container_name: django-web2 # this is the name of the container to us
    build:
      context: ../django
      dockerfile: Dockerfile
    # ports:
    #   - 8000:8000
    # volumes:
    #   - ../django/example:/code

  django-nginx:
    image: nginx:alpine
    container_name: django-nginx
    ports:
      - 8080:8080
    volumes:
      - ./web.conf:/etc/nginx/conf.d/web.conf
    depends_on:
      - django-web2 # this makes sure that the postgres service below has been started prior to attempting to start this service.
```
```bash
$ cd docker/compose
$ docker-compose -f django-with-nginx.yml up -d
$ docker-compose -f django-with-nginx.yml down
```
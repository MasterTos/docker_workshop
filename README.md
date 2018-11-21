# Docker 101

## Prerequisites
* Install [Docker toolbox](https://docs.docker.com/toolbox/overview/#whats-in-the-box) for Windows/Mac
* Create DockerID at [Docker hub](https://hub.docker.com)

## Course outline
* [How to use Docker-machine](https://github.com/MasterTos/docker_workshop#how-to-use-docker-machine)
* [Pull and Push Docker images](https://github.com/MasterTos/docker_workshop#pull-and-push-docker-images)
* [Run container](https://github.com/MasterTos/docker_workshop#run-container)
* [Volume](https://github.com/MasterTos/docker_workshop#volume)
* [Inspect and Log](https://github.com/MasterTos/docker_workshop#inspect-and-log)
* [Dockerfile](https://github.com/MasterTos/docker_workshop#dockerfile)
* [Docker Compose](https://github.com/MasterTos/docker_workshop#docker-compose)

## How to use Docker Machine

After finished then run below command for check docker-machine
```bash
docker-machine --version #check version of docker-machine

docker-machine create --driver virtualbox -virtualbox-memory "1024" docker101 # create new docker-machine

docker-machine ls # checkhttps://github.com/MasterTos/docker_workshop# ip address

#default username/password is docker/tcuser
```

SSH to docker-machine
- `docker-machine ssh docker101` # default ssh via command prompt
- access via putty(windows) to ip address
- access via Shell (mac)

## Pull and Push Docker images

### Pull images
Access to docker-machine via SSH first.

```bash
docker image pull mastertos/<image>
docker images #or docker image ls
```

### Push images
1. Access url: https://hub.docker.com and register
2. Create repositary name `<name>`
3. Tag image to your account name `docker image tag mastertos/<image> <account>/<image>`
4. Login to hub.docker.com
```bash
docker login -u <username>
docker image push <account>/<image>:<tag>
docker logout
```

## Run Container
### Interactive Mode
1. Run docker on interactive mode with command
```bash
docker container run -it --rm --name python -p 8000:8000 \
  mastertos/<image>:<tag> python manage.py runserver
```
2. Open browser with url: `http://<ip address>:8000/` <br>
*You can check ip address with command: `docker-machine ls`*
3. Terminate container: `docker container stop python`
4. With option `--rm` with remove all container after finished work.

### Detach Mode
1. Run docker on detach with command
```bash
docker container run -d --name python -p 8000:8000 \
  mastertos/<image>:<tag> python manage.py runserver
```
2. Open browser with url: `http://<ip address>:8000/`
3. Stop container: `docker container stop python`
4. Remove container: `docker container rm python`
   
### Exec Mode
Syntax: `docker container exec -it <name or id> <command>`
<br>Example:
```bash
docker container exec -it python bash
```

## Volume

## Inspect and Log

## Dockerfile

## Docker Compose
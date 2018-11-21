# Docker 101

## Prerequisites
* Install [Docker toolbox](https://docs.docker.com/toolbox/overview/#whats-in-the-box) for Windows/Mac
* Create DockerID at [Docker hub](https://hub.docker.com)

## Course outline
* [How to use Docker-machine](https://github.com/MasterTos/docker_workshop#how-to-use-docker-machine)
* Pull and Push Docker images
* Run container
* Volume
* Inspect and Log
* Dockerfile
* Docker-compose

## How to use Docker Machine

After finished then run below command for check docker-machine
```bash
docker-machine --version #check version of docker-machine

docker-machine create --driver virtualbox -virtualbox-memory "1024" docker101 # create new docker-machine

docker-machine ls # check ip address

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
1. run docker on interactive mode with command
```bash
docker run -it --rm --name python -p 8000:8000 \
  mastertos/<image>:<tag> python manage.py runserver
```
2. open brower with url: `http://<ip address>:8000/`
3. Terminate container: `docker stop python`
4. with option `--rm` with remove all container after finished work.

### Detach Mode
```bash
docker run -i -d --name python -p 8000:8000 \
  mastertos/<image>:<tag> python manage.py runserver
```

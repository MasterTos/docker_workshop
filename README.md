# Docker 101

## Prerequisites
* Install [Docker toolbox](https://docs.docker.com/toolbox/toolbox_install_windows/) for Windows/Mac
* Create DockerID at [Docker hub](https://hub.docker.com)

## Course outline
* How to use Docker-machine
* Pull and Push image
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
```
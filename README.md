# Simple Containers Management

A simple setup I use to manage docker containers in web application context. It includes [nginx-proxy-manager](https://github.com/jc21/nginx-proxy-manager) and [portainer](https://github.com/portainer/portainer). 

## Principles

* nginx-proxy-manager will be the only service with open ports (80 and 443) on local host
* All the containers must use the `nxpm` network and don't need to publish ports on local host
```yaml
# Example of a docker-compose.yml file with a service joining the existing nxpm network 
version: "3.8"

services:
  myservice:
    image: someimage
    networks:
      - nxpm

networks:
  nxpm:
    external: true
```

## Run the stack

* Change database passwords in `.env` and `production.json`
* You might want to change images version in `.env`
* Bring the containers up :
```Shell
$ docker-compose up -d
```
* Connect to nginx-proxy-manager administration interface on port 81. By default, this interface is only exposed locally on the machine running the docker containers. To remotely accessing the admin interface use ssh tunneling (e.g. `ssh -L 8181:127.0.0.1:81 user@remote_host`) and access the interface on your local machine (e.g. http://localhost:8181)
* Log in to the admin interface with default user/password (admin@example.com/changeme) and update the admin user and password
* You might want to create in nginx-proxy-manager a proxy host for its admin interface (`Forward Hostname : nxpm / Forward Port : 81`) and generate a [Let's Encrypt](https://letsencrypt.org/) certificate for it.
* Create a proxy host for portainer (`Forward Hostname : portainer / Forward Port : 9000`) and generate a [Let's Encrypt](https://letsencrypt.org/) certificate for it.
* You're good to go ! :rocket:

## Documentation

* [Portainer documentation](https://documentation.portainer.io/)
* [Nginx-proxy-manager documentation](https://nginxproxymanager.com/guide/)

## Todo List

- [ ] Add container supervision solution

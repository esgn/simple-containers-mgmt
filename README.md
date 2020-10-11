# Deployment

A simple setup to manage docker instances for web applications. It includes [nginx-proxy-manager](https://github.com/jc21/nginx-proxy-manager) and [portainer](https://github.com/portainer/portainer). 

## Principles

* All the containers must use the `nxpm` network and don't need to expose ports on local host
* nginx-proxy-manager will be the only service with open ports (80 and 443) on local host

## Run the stack

* Change database passwords in `.env` and `production.json`
* You might want to change images version in `.env`
* Bring the containers up :
```
	docker-compose up -d
```
* Connect to nginx-proxy-manager administration interface on port 81. By default, this interface is only exposed locally on the machine running the docker containers. To remotely accessing the admin interface use ssh tunneling (e.g. `ssh -L 8181:127.0.0.1:81 user@remote_host`) and access the interface on your local machine (e.g. http://localhost:8181)
* Log in to the admin interface with default user/password (admin@example.com/changeme) and update the admin user and password
* You might want to create in nginx-proxy-manager a proxy host for its admin interface (`Forward Hostname : nxpm / Forward Port : 81`) and generate a [Let's Encrypt](https://letsencrypt.org/) certificate for it.
* Create a proxy host for portainer (`Forward Hostname : portainer / Forward Port : 9000`) and generate a [Let's Encrypt](https://letsencrypt.org/) certificate for it.
* You're good to go ! :rocket:

## TODO

- [ ] Add container supervision solution

# registry-docker-compose
Customized docker-compose registry stack

## Requirements
* on host that will store the container create this folders :  
`mkdir -p /srv/data/docker/registry/registry /srv/data/docker/registry/certs`  
* and get a (self-signed) certificate:  
`openssl req -newkey rsa:4096 -nodes -sha256 -keyout /srv/data/docker/registry/certs/registry.key -x509 -days 9000 -out /srv/data/docker/registry/certs/registry.crt -subj "/C=IT/ST=Turin/L=Turin/O=Neomediatech/OU=IT Dept/CN=10.40.50.7/subjectAltName=IP:10.40.50.7"`

## Usage
* `curl -L https://raw.githubusercontent.com/Neomediatech/registry-docker-compose/master/docker-compose.yml -o registry-docker-stack.yml`
* `docker stack deploy --compose-file=registry-docker-stack.yml Registry`
* access your Registry stack just deployed with a browser to http://ip-of-your-docker-host:5000

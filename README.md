# registry-docker-compose
Customized docker-compose registry stack

## Requirements
* create this folders :  
`mkdir -p /srv/data/docker/registry/registry /srv/data/docker/registry/certs`  
* get a (self-signed) certificate:  
`openssl req -newkey rsa:4096 -nodes -sha256 -keyout /srv/data/docker/registry/certs/registry.key -x509 -days 9000 -out /srv/data/docker/registry/certs/registry.crt -subj "/C=IT/ST=Turin/L=Turin/O=Neomediatech/OU=IT Dept/CN=10.40.50.7"`


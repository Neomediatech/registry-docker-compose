# registry-docker-compose
Customized docker-compose registry stack

## Requirements
* on host that will store the container create this folders :  
`DIR="/srv/data/docker/registry" ; mkdir -p $DIR/registry $DIR/certs ; chmod 777 $DIR/registry $DIR/certs`  
* and [create|get] a (self-signed) certificate:  
`DIR="/srv/data/docker/registry" ; openssl req -newkey rsa:4096 -nodes -sha256 -keyout $DIR/certs/registry.key -x509 -days 9000 -out $DIR/certs/registry.crt -subj "/C=IT/ST=Turin/L=Turin/O=Neomediatech/OU=IT Dept/CN=10.40.50.7/subjectAltName=IP:10.40.50.7"`

`IP="10.40.50.7" ; DIR="/srv/data/docker/registry" ; openssl req -newkey rsa:4096 -nodes -sha256 -keyout $DIR/certs/registry.key -x509 -days 9000 -out $DIR/certs/registry.crt -subj "/C=IT/ST=Turin/L=Turin/O=Neomediatech/OU=IT Dept/CN=$IP" -extensions SAN -config <(echo -e "[req]\ndistinguished_name = req_distinguished_name\nreq_extensions = v3_req\nprompt = no\n[req_distinguished_name]\n[v3_req]\nkeyUsage = keyEncipherment, dataEncipherment\nextendedKeyUsage = serverAuth\n[SAN]\nsubjectAltName='IP:$IP'")`  
(MAYBE I CAN USE `-extfile` to have a command line more clear)

## Usage
* `curl -L https://raw.githubusercontent.com/Neomediatech/registry-docker-compose/master/docker-compose.yml -o registry-docker-stack.yml`
* `docker stack deploy --compose-file=registry-docker-stack.yml Registry`
* access your Registry stack just deployed with a browser to http://ip-of-your-docker-host:5000

##WARNING (3 oct 2018)
Trying to push an image i get this error
`# docker push 10.40.50.7:5000/clamav-alpine-neo`  
`The push refers to repository [10.40.50.7:5000/clamav-alpine-neo]
Get https://10.40.50.7:5000/v2/: x509: certificate signed by unknown authority (possibly because of "x509: invalid signature: parent certificate cannot sign this kind of certificate" while trying to verify candidate authority certificate "10.40.50.7")`  

I'm investigating on it

(https://10.40.50.7:40443/home)

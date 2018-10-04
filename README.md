# registry-docker-compose
Customized docker-compose registry stack

## Requirements
* on host that will store the container create this folders :  
`DIR="/srv/data/docker/registry" ; mkdir -p $DIR/registry $DIR/certs ; chmod 777 $DIR/registry $DIR/certs`  
* and [create|get] a (self-signed) certificate:  
`DIR="/srv/data/docker/registry" ; openssl req -newkey rsa:4096 -nodes -sha256 -keyout $DIR/certs/registry.key -x509 -days 9000 -out $DIR/certs/registry.crt -subj "/C=IT/ST=Turin/L=Turin/O=Neomediatech/OU=IT Dept/CN=10.40.50.7/subjectAltName=IP:10.40.50.7"`

```
IP="10.40.50.7"  
DIR="/srv/data/docker/registry"  
TMP_SSL_FILE=$(echo -n "openssl-" ; < /dev/urandom tr -dc _A-Z-a-z-0-9 | head -c${1:-32};echo ".cnf";)  

cat <<EOF > $TMP_SSL_FILE  
[req]  
distinguished_name = req_distinguished_name  
req_extensions = v3_req  
prompt = no  
[req_distinguished_name]  
countryName            = IT  
stateOrProvinceName    = Torino  
localityName           = Torino  
organizationName       = Neomediatech  
organizationalUnitName = IT Dept  
commonName             = $IP  
[v3_req]  
keyUsage = keyEncipherment, dataEncipherment  
extendedKeyUsage = serverAuth  
[SAN]  
subjectAltName='IP:$IP'  
EOF   

openssl req -newkey rsa:4096 -nodes -sha256 -keyout $DIR/certs/registry.key -x509 -days 9000 -out $DIR/certs/registry.crt -extensions SAN -config $TMP_SSL_FILE  
rm -f $TMP_SSL_FILE
```  

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

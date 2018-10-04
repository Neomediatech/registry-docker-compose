# registry-docker-compose
Customized docker-compose registry stack

## Requirements
* I tried to run Registry with a self-signed certificate, but I fell into a black hole. No way to tell docker to accept the cert. Dozen of sites give you "THE solution", but none of these were usefull for me. Maybe I cannot understand something.  
Anyway, my server aren't exposed over Internet and no one other than trusted users acces our servers. Then I opted for a insecure registry configuration: https://docs.docker.com/registry/insecure/#deploy-a-plain-http-registry  
Basically you have to do this:  
* `vi /etc/docker/daemon.json` (maybe the file doesn't exists, no problem) and add the section  
```
{
  "insecure-registries" : ["10.40.50.7:5000"]
}
```
* save & close
* service docker restart

## Usage
* `curl -L https://raw.githubusercontent.com/Neomediatech/registry-docker-compose/master/docker-compose.yml -o registry-docker-stack.yml`
* `docker stack deploy --compose-file=registry-docker-stack.yml Registry`
* access your Registry stack just deployed with a browser to http://ip-of-your-docker-host:5000

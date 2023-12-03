# An Nginx container to act as a Reverse Proxy
### Create dir called proxy
```
mkdir proxy
```
```
cd proxy
```
### create another directories called certs and conf inside proxy dir.
```
mkdir certs
```
```
mkdir conf
```
### Move into certs to generate a ssl
```
cd /proxy/certs
```
### We have 2 option to generate certs(I prefer option one)
(option 1)
```
openssl req -x509 -newkey rsa:4096 -keyout localhost.key -out localhost.crt -days 365 -nodes
```
(option 2)
```
apt install libnss3-tools -y
```
```
apt install mkcert
```
```
   mkcert \
  -cert-file esales.mymarket.com.crt \
  -key-file esales.mymarket.com.key \
   esales.mymarket.com
```
go back to default dir
```
cd ..
```
```
nano docker-compose.yml 
```
```
  GNU nano 6.2                                                                                                  docker-compose.yml
version: '3'

services:
  proxy:
    image: nginx:1.19.10-alpine
    ports:
      - 80:80
      - 443:443
    volumes:
      - ./proxy/conf/nginx.conf:/etc/nginx/nginx.conf
      - ./proxy/certs:/etc/nginx/certs
    depends_on:
      - nopcom

  nopcom:
    image: nopweb
    container_name: nopcom
    ports:
      - "8082:80"
```
### now Go to conf dir
```
cd /proxy/conf/
```
```
nano nginx.conf

events {
  worker_connections 1024;
}

http {
  server {
    listen 80;
    server_name esales.mymarket.com;
    return 301 https://$host$request_uri;
  }

  server {
    listen 443 ssl;
    server_name esales.mymarket.com;

    ssl_certificate /etc/nginx/certs/esales.mymarket.com.crt;
    ssl_certificate_key /etc/nginx/certs/esales.mymarket.com.key;
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
    ssl_ciphers HIGH:!aNULL:!MD5;

    location / {
      proxy_buffering off;
      proxy_set_header X-Forwarded-Proto $scheme;
      proxy_set_header X-Forwarded-Host $host;
      proxy_set_header X-Forwarded-Port $server_port;

      proxy_pass http://nopcom;
    }
  }
}
```
### and at the end we call the docker-compose
```
docker compose up
```

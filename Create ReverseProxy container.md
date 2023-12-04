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
version: '3'

services:
  proxy:
    image: nginx:1.19.10-alpine
    ports:
      - 80:80
      - 443:443
    volumes:
      - ./nginx/nginx.conf:/etc/nginx/nginx.conf:ro
      - ./ssl/site.crt:/etc/ssl/certs/site.crt
      - ./ssl/site.dec.key:/etc/ssl/private/site.dec.key

  nopcom:
       depends_on:
         - proxy
       image: nopweb
    container_name: nopcom
    ports:
      - "8081:8081"
```
### now Go to conf dir
```
cd /proxy/conf/
```
nano nginx.conf
```
worker_processes 1;

events { worker_connections 1024; }

http {

    sendfile on;
    large_client_header_buffers 4 32k;

    server {
        listen 80;
        server_name my-site;

        location / {
            return 301 https://$host$request_uri;
        }
    }

    server {
        listen 443 ssl;
        server_name my-site;

        ssl_certificate /etc/ssl/certs/my-site.crt;
        ssl_certificate_key /etc/ssl/private/my-site.key;

        location / {
            proxy_pass         http://{myLocalIp}:8081;
            proxy_redirect     off;
            proxy_http_version 1.1;
            proxy_cache_bypass $http_upgrade;
            proxy_set_header   Upgrade $http_upgrade;
            proxy_set_header   Connection keep-alive;
            proxy_set_header   Host $host;
            proxy_set_header   X-Real-IP $remote_addr;
            proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header   X-Forwarded-Proto $scheme;
            proxy_set_header   X-Forwarded-Host $server_name;
            proxy_buffer_size           128k;
            proxy_buffers               4 256k;
            proxy_busy_buffers_size     256k;
        }
    }
```
### and at the end we call the docker-compose
```
docker compose up
```

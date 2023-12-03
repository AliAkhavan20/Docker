# Nexus Private Registry with no ssl and ssl

run your Nexus container securely over HTTPS, you can follow these steps. In this example, I'll assume you have a certificate file (your_certificate.crt) and a private key file (your_private_key.key). Also, replace your_domain.com with your actual domain.

### Create a Docker Network (Optional):
Create a Docker network to isolate Nexus from other containers:
```
docker network create nexus-network
Run Nexus Container with SSL:
Run the Nexus container, mounting your SSL certificate and private key:
```
### make a docker-compose.yaml file
```
version: "3.9"
services:
  nexus:
    image: sonatype/nexus3
    ports:
    - "8081:8081"
    - "8082:8082"
    volumes:
    - ./host-nexus-data:/nexus-data
```
### give permission to UID nexus user inside the container
```
sudo chown -R 200 nexus-data/
```
### Run the script
```
docker-compose up
```
## Configure Nexus with SSL and a proper cert:

> first follow the instructions on "Create ReversProxy container" and run your nexus in the docker-compose file created in the reverseproxy document

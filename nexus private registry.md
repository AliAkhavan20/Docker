# Nexus Private Registry

run your Nexus container securely over HTTPS, you can follow these steps. In this example, I'll assume you have a certificate file (your_certificate.crt) and a private key file (your_private_key.key). Also, replace your_domain.com with your actual domain.

## Create a Docker Network (Optional):
Create a Docker network to isolate Nexus from other containers:
```
docker network create nexus-network
Run Nexus Container with SSL:
Run the Nexus container, mounting your SSL certificate and private key:
```
```
docker run -d -p 8081:8081 --name nexus --network nexus-network \
  -v /path/to/your_certificate.crt:/nexus-data/ssl/your_certificate.crt \
  -v /path/to/your_private_key.key:/nexus-data/ssl/your_private_key.key \
  -e INSTALL4J_ADD_VM_PARAMS="-Djavax.net.ssl.keyStore=/nexus-data/ssl/your_certificate.crt -Djavax.net.ssl.keyStorePassword=changeit -Djavax.net.ssl.trustStore=/nexus-data/ssl/your_certificate.crt -Djavax.net.ssl.trustStorePassword=changeit" \
  sonatype/nexus3
```
> Replace /path/to/your_certificate.crt and /path/to/your_private_key.key with the actual paths to your certificate and private key files.

## Configure Nexus SSL:

After the container is running, access the Nexus web interface at https://localhost:8081 (or replace localhost with your server's IP).

Log in with the default credentials (admin/admin123).
Go to "Administration" > "Security" > "SSL Certificates".
Provide the path to your SSL certificate and private key files. Save the changes.
Restart Nexus:
Restart the Nexus container to apply the SSL configuration changes:
```
docker restart nexus
```
Access Nexus via HTTPS:
Nexus should now be accessible via HTTPS. Open your browser and go to https://localhost:8081 (or the appropriate URL).

Note: The environment variable INSTALL4J_ADD_VM_PARAMS is used to pass Java Virtual Machine parameters to enable SSL in Nexus. In this example, it's configuring the keystore and truststore locations. Ensure that you've replaced your_certificate.crt, and adjust passwords if necessary.

Always remember to adapt the commands based on your specific requirements and environment. Additionally, consider using a reverse proxy (like Nginx or Apache) in front of Nexus for enhanced security and to manage SSL termination.
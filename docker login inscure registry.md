# Deploy a plain HTTP registry

> Warning: It’s not possible to use an insecure registry with basic authentication.
> This procedure configures Docker to entirely disregard security for your registry. This is very insecure and is 
not recommended.
> It exposes your registry to trivial man-in-the-middle (MITM) attacks.
> Only use this solution for isolated testing or in a tightly controlled, air-gapped environment.

Edit the daemon.json file, whose default location is /etc/docker/daemon.json on Linux or 
C:\ProgramData\docker\config\daemon.json on Windows Server. If you use Docker Desktop for Mac 
or Docker Desktop for Windows, click the Docker icon, choose Settings and then choose Docker Engine.

If the daemon.json file does not exist, create it. Assuming there are no other settings in the file, 
it should have the following contents:

```
{
  "insecure-registries" : ["myregistrydomain.com:5000"]
}
```

Substitute the address of your insecure registry for the one in the example.

With insecure registries enabled, Docker goes through the following steps:

First, try using HTTPS.
If HTTPS is available but the certificate is invalid, ignore the error about the certificate.
If HTTPS is not available, fall back to HTTP.
Restart Docker for the changes to take effect.
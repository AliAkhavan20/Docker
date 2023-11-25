# Create Container registry with docker-Registry

## Pull docker container registry images 
```
docker pull registry:2
```
```
docker run -d -p 5000:5000 --name local-registry -v /path/to/config.yml:/etc/docker/registry/config.yml registry:2
```
```
docker push localhost:5000/your-image-name
```
## Verify the pushed image:
> You can verify that your image is in the local registry by using the Docker Registry API directly or by browsing
to http://localhost:5000/v2/_catalog in your web browser. This URL will display a list of repositories stored in 
the local container registry.

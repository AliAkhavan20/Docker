# check whats inside image:
```
docker images ls
```
```
docker run -it --entrypoint /bin/bash general-solution-image
```
# check whats inside container:
```
docker container ls
```
```
docker run -it --entrypoint /bin/bash <CONTAINER_ID>
```


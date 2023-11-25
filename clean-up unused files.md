# Clean-up unused files
To control the storage in your os you must delete the unused docker images or containers were built before.
```
docker volume rm $(docker volume ls -qf dangling=true)
```
```
docker system prune 
```
```
docker system prune -a -f
```
```
du -shc /var/lib/docker/overlay2/*/diff
```
```
systemctl restart docker   ---> clear overlay2
```
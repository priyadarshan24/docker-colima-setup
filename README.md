# docker-lima-setup
The following github repository contains steps to run docker container without Docker Desktop on Mac using colima (https://github.com/abiosoft/colima)

```
# Steps to install Colima:
brew install colima

//Start Colima with defaults
colima start

//If you are using TestContainers to spawn docker container to run database Integration tests. Perform the following additional steps:
//As of Colima v0.4.0 it's recommended to set the active Docker context to use Colima. After the context is set Testcontainers for Go will automatically be //configured to use Colima.
docker context use colima

//Post that check if docker context has been changed to point to Colima
docker context ls

```
You should see the following (as shown in image below) when you hit the "docker context ls" with asterik remark indicating that the default docker context is Colima

<img width="729" alt="image" src="https://user-images.githubusercontent.com/100334324/228026026-b000be6a-4868-453f-bb03-8622023e45e4.png">

Check following article for more details on how to set Colima has default docker context - https://golang.testcontainers.org/system_requirements/using_colima/

For More advanced settings refer the shared GitHub Repository of Colima ((https://github.com/abiosoft/colima)

# Steps to ensure Volume Mounting works fine with Colima
* With volume mappings and mountings, it is important that the uid/gid of the owner of the volume directory to be mounted is identical outside and inside the container.

* Following definition need to be added in the docker-compose file:
```
 user: "${UID}:${GID}"
 ```
* Additionally add the following exports in your .bashrc or .zshrc file
```
export GID=$(id -g)
export UID=$(id -u)
```

* Ensure that the volume directory to be mounted from the host machine in the container is created with the same UID/GID which is setup in the docker compose file. If required make the directory manually before starting the docker compose. If the uid/gid do not have read write permissions on the directory, then even container users would inherit the same privelges and container wont startup due to lack of permissions 

* With the above set of changes, the files in the host would be accesible and writeable by the container

I have added a sample docker compose to run postgres docker container with colima and without docker desktop

### Please note that there might be better way to set all this up. This one I could get it working. Feel free to explore alternate and better ways to get this up and running & contribute to this repository


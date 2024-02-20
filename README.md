# Compose RM reproduction of the issue


Steps to recreate the issue:

Normal behaviour:
````bash
# start repo
docker compose up -d
# down containers
docker compose down --remove-orphans
# check lefover containers
docker ps -a
# -> there are no leftover containers from the compose
````

broken behaviour with manually started profile container


````bash
# start repo
sudo docker compose up -d
# start magic
sudo docker compose up magic
# down containers
sudo docker compose down --remove-orphans
# check leftover containers
sudo docker ps -a
# -> there are leftover containers from the compose future starts will die with network issues
````
````output
dev@dev:~/compose-rm-test$ sudo docker ps -a
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS                      PORTS     NAMES
cffa0cf98dd8   ubuntu    "getent hosts sorcery"   38 seconds ago   Exited (2) 38 seconds ago             compose-rm-test-magic-1
````


example output from starting the container again after the down command
````output
dev@dev:~/compose-rm-test$ sudo docker compose up magic
[+] Running 3/0
 ✔ Network compose-rm-test_database_network  Created                                                                                                  0.0s
 ✔ Container compose-rm-test-sorcery-1       Created                                                                                                  0.0s
 ✔ Container compose-rm-test-magic-1         Created                                                                                                  0.0s
Attaching to magic-1
Error response from daemon: network 3714a86429c42c0fc724a0b6d0b38a9d88fce1c4efc88c230ee26438444c995e not found
````

Expected behaviour:

down --remove-orphans should remove all containers from the compose file and not leave any leftovers


## Testing older docker versions

error exists
````bash
VERSION_STRING=5:24.0.9-1~ubuntu.20.04~focal
COMPOSE_VERSION=2.24.5-1~ubuntu.20.04~focal
sudo apt-get install docker-ce=$VERSION_STRING docker-ce-cli=$VERSION_STRING containerd.io docker-buildx-plugin docker-compose-plugin=$COMPOSE_VERSION
````

the docker version is as in our old actions logs
````bash
VERSION_STRING=5:24.0.6-1~ubuntu.20.04~focal
COMPOSE_VERSION=2.24.1-1~ubuntu.20.04~focal
sudo apt-get install docker-ce=$VERSION_STRING docker-ce-cli=$VERSION_STRING containerd.io docker-buildx-plugin docker-compose-plugin=$COMPOSE_VERSION
````

correct behavior
````bash
VERSION_STRING=5:24.0.6-1~ubuntu.20.04~focal
COMPOSE_VERSION=2.21.0-1~ubuntu.20.04~focal
sudo apt-get install docker-ce=$VERSION_STRING docker-ce-cli=$VERSION_STRING containerd.io docker-buildx-plugin docker-compose-plugin=$COMPOSE_VERSION
````


````bash
VERSION_STRING=5:24.0.6-1~ubuntu.20.04~focal
COMPOSE_VERSION=2.20.2-1~ubuntu.20.04~focal
sudo apt-get install docker-ce=$VERSION_STRING docker-ce-cli=$VERSION_STRING containerd.io docker-buildx-plugin docker-compose-plugin=$COMPOSE_VERSION
````

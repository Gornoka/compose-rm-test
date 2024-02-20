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

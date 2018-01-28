Docker Commands
************************************

This part contains a collection of basic and useful Docker commands in daily using. A full list of Docker commands can be found at `here <https://docs.docker.com/engine/reference/commandline/docker/>`_.

===============   ======================================================
docker images         List images on the system
docker ps             List containers that are running now
docker pull           Pull an image or a repository from a registry
docker push           Push an image or a repository to a registry
docker run            Run a command in a new container
docker exec	      Run a command in a running container
docker rm	      Remove one or more containers
docker rmi	      Remove one or more images
===============   ======================================================

There are two usual ways to exit a container::

 exit or ctrl+D        Exit a container and close it, then container can't be found via docker ps
 ctrl+P+Q              Exit a container without closing it, container can be found via docker ps

In most cases, people use ctrl+P+Q to exit a container because jobs running in it will not be killed.

To resume a container after exiting with ctrl+P+Q::

 docker exec -it containerID /bin/bash

or ::

 docker attach containerID

Note: Using the second one will sometimes get stuck and have no response for a long time, and the first one is preferred. 

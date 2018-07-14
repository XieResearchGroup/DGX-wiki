Docker Commands
************************************

Besides the Docker commands mentioned in previous part for `administrators <http://dgx-wiki.readthedocs.io/en/latest/docs/docker/service.html#pull-docker-images-from-ngc-for-administrators>`_ and `users <http://dgx-wiki.readthedocs.io/en/latest/docs/docker/service.html#use-docker-images-for-users>`_. This part contains a collection of basic and useful Docker commands in daily using. A full list of Docker commands can be found at `here <https://docs.docker.com/engine/reference/commandline/docker/>`_.

Some common use commands:

===============   ======================================================
docker images         List images on the system
docker ps             List containers that are running now
docker pull           Pull an image or a repository from a registry
docker push           Push an image or a repository to a registry
docker run            Run a command in a new container
docker rename         Rename a container
docker kill           Kill one or more running containers 
===============   ======================================================

There are two usual ways to exit a container. In most cases, people use "ctrl+P+Q" to exit a container and jobs running in it will not be killed::

 exit or ctrl+D        Exit a container and close it, then container cannot be found via docker ps
 ctrl+P+Q              Exit a container without closing it, container can be found via docker ps

To resume a container after exiting with "ctrl+P+Q"::

 docker exec -it containerNAME /bin/bash

or ::

 docker attach containerNAME

Note: Using the second one will sometimes get stuck and have no response for a long time. The first one is preferred. 

---
date: 2015-02-13 04:20
tags: Minecraft Docker MacOS
title: Run a Minecraft Server on OSX Boot2Docker
---

##  Run a Minecraft Server on OSX Boot2Docker

Here's how I did it. I hope you find it useful!

##  One-time Setup

Do these steps once, to initialize Boot2Docker:

Step 1: Install [Docker for OS X](https://docs.docker.com/installation/mac/)

Step 2: Create a directory to hold your Minecraft files. This needs to be
under the /Users part of your file system because boot2docker automatically
mounts /Users to the boot2docker-vm.

  mkdir /Users/yourname/minecraft/data

Step 3: Initialize boot2docker

  boot2docker init

Step 4: Forward the TCP port Minecraft uses from the Mac to the boot2docker-vm.

  VBoxManage modifyvm "boot2docker-vm" --natpf1 "tcp-port25565,tcp,,25565,,25565";

##  Start Minecraft

Do these steps every time you want to start your Minecraft server.

Step 1: Start boot2docker.

 boot2docker start

Step 2: Set up the shell variables so you can use the docker command.

 $(boot2docker shellinit)

Step 3: Run the minecraft container.

 CONTAINER=$(docker run -v /Users/yourname/minecraft/data:/data -d -e EULA=TRUE -e VERSION=LATEST -p 25565:25565 itzg/minecraft-server)

The first time your run this it will take a few minutes to download and
install minecraft. After that it should be much faster

##  View the Minecraft Server Log

  docker logs $CONTAINER

This prints out the logs from the container (you set the CONTAINER variable as
part of the docker run command above.)

If you've lost track of your container, you can list all currently running
containers.

  docker ps

If you don't see any containers, you container may have already exited. The
Minecraft server will exit if it encounters an error while running.

##  Shut Down

You can shut down all running containers and quit boot2Docker by using the
stop command:

  boot2docker stop

Note that the Minecraft Server files will be stored in
/Users/yourname/minecraft/data, and when you've stopped the server you can
edit the files using your mac. (You might want to edit the files in order to
modify the server settings.)

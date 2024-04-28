# Let's Install a dedicated docker server

## Video:

[![Let's Install a dedicated docker server](https://img.youtube.com/vi/Lz_v5guw4rI/0.jpg)](https://www.youtube.com/watch?v=Lz_v5guw4rI)

## Links discussed in the video:
- [Debian ISO download](https://www.debian.org/distrib/)
- [Raspberry Pi Imager Download](https://www.raspberrypi.com/software/)
- [balenaEtcher Download](https://etcher.balena.io)
- [Install the Docker Engine on Debian](https://docs.docker.com/engine/install/debian/)
- [Install the Docker Compose plugin](https://docs.docker.com/compose/install/linux/#install-using-the-repository)
- [Portainer](https://www.portainer.io)
- [Portainer CE](https://github.com/portainer/portainer)

## Terminal commands from the video

### Installing Debian Linux

Get IP Address from command line

    ip addr

Login using SSH from Terminal on Windows 11 or MacOS, replace [username] with your user name and [ip address] with the server's IP address.

    ssh [username]@[ip address]

### Configuring Debian Linux - Configuring sudo

Installing the sudo package.

    apt install sudo

Adding the username to the sudo group, replace [username] with your user name 

    /sbin/adduser [username] sudo.

### Configuring Debian Linux - Configuring the IP Address

Backing up the network file

    sudo cp /etc/network/interfaces /root/

Editing the network file

    sudo nano /etc/network/interfaces

Insert text in the network file, remember to change the IP configuration to match your network

    iface enp0s5 inet static
        address 192.168.0.xxx
        netmask 255.255.255.0
        gateway 192.168.0.1
        dns-domain debian-vm.local
        dns-nameservers 8.8.8.8 1.1.1.1 192.168.0.1

Restarting the network interface

    sudo systemctl restart networking.service

## Docker

### Installing Docker

Add the official GPG key

    sudo apt-get update
    sudo apt-get install ca-certificates curl
    sudo install -m 0755 -d /etc/apt/keyrings
    sudo curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc
    sudo chmod a+r /etc/apt/keyrings/docker.asc

Add the Docker repository to the apt sources file

    echo \
    "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/debian \
    $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
    sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
    sudo apt-get update

Install Docker CE

    sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

Test Docker

    sudo docker run hello-world

Create the Docker group

    sudo groupadd docker

Add your user to the group

    sudo usermod -aG docker $USER

### Installing Docker Compose, if it is missing

Update the linux repositories

     sudo apt-get update

Install the docker compose plugin

    sudo apt-get install docker-compose-plugin

Creating a folder fro all our docker data

    sudo mkdir docker

Creating a folder for Portainer

    cd docker
    sudo mkdir portainer

## Installing Portainer

Creating our docker compose file for portainer

    sudo nano docker-compose.yml

Copy the content of the [Docker Compose File](./docker_compose.yml) to the docker compose file on your server

Install Portainer

    sudo docker compose up

## Post install

To get the Portainer ID

    sudo docker ps

To restart Portainer, replace [PortainerId] with the Id from the previous command

    sudo docker restart containerid

    

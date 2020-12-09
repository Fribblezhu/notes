## Official Guide Page

<https://docs.docker.com/engine/install/centos/>

## Require
* container-selinux*.rpm
* container.io*.rpm
* docker-ce*.rpm
* docker-ce-cli*.rpm

## Install Command

```
rpm -ivh container-selinux*.rpm container.io*.rpm docker-ce*.rpm docker-ce-cli.rpm
```

## Manager Docker as an non-root user

* [official guide](https://docs.docker.com/engine/install/centos/)


#### 1. Create the docker group.

```
$ sudo groupadd docker
```
#### 2.Add your user to the docker group.
```
$ sudo usermod -aG docker $USER
```
#### 3 Flush
Log out and log back in so that your group membership is re-evaluated.

If testing on a virtual machine, it may be necessary to restart the virtual machine for changes to take effect.

On a desktop Linux environment such as X Windows, log out of your session completely and then log back in.

On Linux, you can also run the following command to activate the changes to groups:
```
$ newgrp docker
```
#### Verify that you can run docker commands without sudo.
```
$ docker run hello-world
```



## Configuration Dictionary

* `/etc/docker/`

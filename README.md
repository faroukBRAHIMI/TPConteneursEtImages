
# Premier TP Docker - Conteneurs et images

Pour créer le fork : https://classroom.github.com/a/bsumLYBx


# Table of Contents
- [Docker Versions and Information](#docker-versions-and-information)
- [Docker Service and Socket](#docker-service-and-socket)
- [Docker hub](#docker-hub)



# Docker Versions and Information

running ```docker info``` gives helpful informations concerning the docker images, running containers, volumes (which are used to persist data), and overall configuration

## Client Information
| **Component**      | **Version** |
|--------------------|-------------|
| Docker Engine      | 27.3.1      |
| Context            | default     |
| Debug Mode         | false       |

### Installed Plugins
| **Plugin**         | **Version** | **Path**                                          |
|--------------------|-------------|---------------------------------------------------|
| Docker Buildx      | v0.17.1     | /usr/libexec/docker/cli-plugins/docker-buildx     |
| Docker Compose     | v2.29.7     | /usr/libexec/docker/cli-plugins/docker-compose    |

## Server Information
| **Component**          | **Details**                                      |
|------------------------|--------------------------------------------------|
| Server Version         | 27.3.1                                           |
| Containers             | 1                                                |
| Running                | 0                                                |
| Paused                 | 0                                                |
| Stopped                | 1                                                |
| Images                 | 1                                                |

### Storage Driver
| **Storage Driver**      | overlay2                                         |
|-------------------------|--------------------------------------------------|
| Backing Filesystem      | extfs                                            |
| Supports d_type         | true                                             |
| Using metacopy          | false                                            |
| Native Overlay Diff     | true                                             |
| userxattr               | false                                            |

### Logging and Cgroup
| **Logging Driver**      | json-file                                        |
|-------------------------|--------------------------------------------------|
| Cgroup Driver           | systemd                                         |
| Cgroup Version          | 2                                                |

### Installed Plugins
| **Plugin**             | **Available Options**                           |
|------------------------|-------------------------------------------------|
| Volume                 | local                                           |
| Network                | bridge, host, ipvlan, macvlan, null, overlay   |
| Log                    | awslogs, fluentd, gcplogs, gelf, journald, json-file, local, splunk, syslog |

### Runtime Information
| **Runtime**            | io.containerd.runc.v2, runc                    |
|------------------------|-------------------------------------------------|
| Default Runtime        | runc                                           |
| Init Binary            | docker-init                                    |

### Version Information
| **containerd Version** | 57f17b0a6295a39009d861b89e3b3b87b005ca27        |
|------------------------|-------------------------------------------------|
| runc Version           | v1.1.14-0-g2c9f560                              |
| init Version           | de40ad0                                         |

### Security Options
| **Security Option**     | **Details**                                    |
|-------------------------|-------------------------------------------------|
| apparmor                | Enabled                                         |
| seccomp                 | Profile: builtin                               |
| cgroupns                | Enabled                                         |

## System Information
| **Component**           | **Details**                                      |
|-------------------------|--------------------------------------------------|
| Kernel Version          | 5.15.0-118-generic                              |
| Operating System        | Ubuntu 22.04.4 LTS                              |
| OSType                  | linux                                           |
| Architecture            | x86_64                                          |
| CPUs                    | 2                                                |
| Total Memory            | 1.918 GiB                                       |
| Docker Root Dir         | /var/lib/docker                                 |
| Debug Mode              | false                                           |
| Experimental            | false                                           |

### Insecure Registries
| **Registry**            | 127.0.0.0/8                                     |

### Swarm and Live Restore
| **Swarm**               | inactive                                         |
|-------------------------|--------------------------------------------------|
| Live Restore Enabled    | false                                           |




# Docker Service and Socket

## Docker service

To see the docker service, run this command ```sudo systemctl list-units --type=service | grep docker```. You should see ```docker.service```

It runs the Docker daemon (dockerd). The Docker daemon is the background process responsible for managing Docker containers, images, networks, and volumes. It listens for requests (usually through a socket) and executes commands such as docker run, docker build, and docker ps.

## Docker socket

To see the docker service, run this command ```sudo systemctl list-units --type=socket | grep docker```. You should see ```docker.socket```

It acts as a communication channel with the docker daemon. 

## Docker users

We have the ability to see the user that started docker daemon, by running this command ```ps aux | grep dockerd``` The output given should be similar to this ```root        8538  0.0  3.9 2060888 79860 ?       Ssl  09:06   0:01 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock```. In my case docker was initialized by root. 

## Docker lifecycle

### Stopping docker
To stop docker run this command ```ystemctl stop docker``` . You might be asked to authenticate to complete the action. The output should be something like this.

``` 
==== AUTHENTICATION COMPLETE ===
Warning: Stopping docker.service, but it can still be activated by:
  docker.socket
  ```

### Socket Status after stopping docker.service

Stopping docker.service doesnt mean necessarily that docker.socket is gonna get stopped. Since they work seperately from each other, the docker socket stays active and listening for new connections. But the actions received are not gonna be executed since the service is stopped. To get the socket status type ```systemctl status docker.socket``` You should get something similar to this 
``` 
● docker.socket - Docker Socket for the API
     Loaded: loaded (/lib/systemd/system/docker.socket; enabled; vendor preset: enabled)
     Active: active (listening) since Thu 2024-11-14 09:06:24 UTC; 1h 6min ago
   Triggers: ● docker.service
     Listen: /run/docker.sock (Stream)
      Tasks: 0 (limit: 2219)
     Memory: 0B
        CPU: 701us
     CGroup: /system.slice/docker.socket
```

### Deactivate/Reactivate Docker

To deactivate docker, type this command ```sudo systemctl disable docker``` . This should stop docker from starting automatically when the system starts.

To reverse this, replace ```disable``` with ```enable```


### Create a group

We can avoid using sudo each time we run docker command in terminal by creating a group and giving it the proper exec rights.

we'll need to run these commands

- create a group ```sudo groupadd docker```
- ```sudo su - dockeruser```
- add dockeruser to docker group ```sudo usermod -aG docker dockeruser```
- reboot VM
- test by running ```docker --version``` you should see the installed version of docker 

### Where does docker stock its data, containers and such ?

Docker stocks everything it needs in ```/var/lib/docker```
the output gives something like this :
```
.   buildkit    engine-id  network   plugins   swarm  volumes
..  containers  image      overlay2  runtimes  tmp 
```

### Where are the containers stored ?

In ```/var/lib/docker/containers``` for a fresh install of docker, you should find only one container for the default hello world example.

### How to get existing images ?

You'll need to run this command ```docker image ls``` for a fresh install you might only find the hello-world image, the command should give something like this : 
```
REPOSITORY    TAG       IMAGE ID       CREATED         SIZE
hello-world   latest    d2c94e258dcb   19 months ago   13.3kB
```

We can also look for an image out of the existing images published in docker hub, for example for the default hello-world image we run ```docker search hello-world``` it should give something like this:
```
NAME                                DESCRIPTION                                     STARS     OFFICIAL
hello-world                         Hello World! (an example of minimal Dockeriz…   2355      [OK]
rancher/hello-world                 This container image is no longer maintained…   6
...
```

### How to run an image ? 

You can run an image using its name directly, if the image doesnt exist locally, docker will fetch it automatically. Run this command to run the most starred hello-world image ```docker run hello-world```. The output should look something like this : 
```
Hello from Docker!
This message shows that your installation appears to be working correctly.    

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.     
    (amd64)
...
```

### Does docker reuse the same container each time we run the image

When working with the same image, ```hello-world``` in this case. docker generates a new instance for it each time, using ```docker ps -a``` shows a new instance with its hash, a new folder with the same hash gets added to the containers folder inside ```/var/lib/docker/containers```, output should look something like this, instance hash strings might change for your case!
```
125e710643bceef06e374fa0d0a7dd30def9b82e2fbb0839cd1da09ea2bc91aa  663503c099740ee20a0d5765c21839a655a9271e917ad5218351acceaf399036
218e7b3db7a5c9fd9e4f6e1eaac3479b4cac06e0364dc82e2b9d93cfef87bd6e  8aeeefd515eadeb2f4cb7c3100f8e0a0d699807882c433680ee765c5a9da03cb
34d11b51f4258cf6dc07e861af8efda2020a740a5a030301e758535de7abe540  b11ca41f94db3bb418651859252a1c64f60d03dc5504ed71fb155d2f442be9b9
584fd25325e3091a8f031ae753d0c4793ff9a527c93835415a34353fa8b48669
```
Its important to note that even though we have multiple instances of the hello-world application, but only one image was used (the prototype). running ```docker images``` should give us this : 
```
REPOSITORY    TAG       IMAGE ID       CREATED         SIZE  
hello-world   latest    d2c94e258dcb   19 months ago   13.3kB
```

### Whats the hash string under IMAGE ID when listing the images

Docker generates an SHA256 hash to uniquely identify an image/container/volume...etc

### How can i get the running containers ?

Run this command to get all running containers ```docker ps```. We can also use this command to get the number of currently running containers ```docker ps -q | wc -l```, the ```-q``` gets back only the hash strings of the containers. The output should be a number.

Note: ```docker ps``` is an alias for this command ```docker container ls```

### How to get the ID of currently used image

Run this command to list the images ```docker images```
this should give you a list of existing images, IMAGE ID holds the UID of the image, output should look something like this: 
```
REPOSITORY    TAG       IMAGE ID       CREATED         SIZE
hello-world   latest    d2c94e258dcb   19 months ago   13.3kB
```

### Does docker download the image each time we start a container of it ?

No, docker only downloads the image the first time its used, then, it stores it locally.





# Docker hub

### What does docker contain ?

Docker hub contains official images maintained by organizations (docker, ubuntu...etc). But it equally
contains images deployed by simple users of the platform. each image is referenced
by a tag. the image can be stared (a sort of rating). and the image's maintainer/s

### How to tell an official image

An official image will have a certified badge next to its name, lets take 
ubuntu's official image for example [Ubuntu official image](https://hub.docker.com/_/ubuntu)


### Image name strcuture explained

Lets take the ubuntu official images for example, follow this [link](https://hub.docker.com/_/ubuntu)
Inside you should see a list of available images of ubuntu, something like this 

```
20.04, focal-20241011, focal⁠

22.04, jammy-20240911.1, jammy⁠

24.04, noble-20241118.1, noble, latest⁠

24.10, oracular-20241120, oracular, rolling⁠

25.04, plucky-20241124, plucky, devel⁠

<ubuntu OS version>, <specific build tag>, <release alias>

```

For the release tags : Focal Ubuntu 20.04 LTS | Jammy Ubuntu 22.04 LTS ...etc
For release aliases we have : 
- Rolling : latest non-LTS release often used in Docker for projects that need the most recent features and updates.
- Devel : latest development build, used for testing and experimentation with the next version of Ubuntu
- Latest : most recent LTS release, useful when you always want the most up-to-date stable version.



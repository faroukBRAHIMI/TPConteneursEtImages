
# Premier TP Docker - Conteneurs et images

Pour créer le fork : https://classroom.github.com/a/bsumLYBx


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


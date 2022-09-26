# dk-cisco-26sep2022

## Training Plan

<img src="plan.png">

### bare-metal systems if there is incompatiblity in apps 

<img src="baremetal.png">

### Solution by Hypervisor -- Virtualization 

<img src="vm.png">

### problems with Vm 

<img src="vmprob.png">

### Understanding components of OS 

<img src="os.png">

### Introduction to containers 

<img src="cont.png">

### vm vs containers 

<img src="cont2.png">

### FInal journey to the containers 

<img src="cj.png">

### Docker installation link

[click_here](https://docs.docker.com/engine/install/)

### installing docker on amazon linux 

```
[root@ip-172-31-91-4 ~]# yum  install docker -y 
Failed to set locale, defaulting to C
Loaded plugins: extras_suggestions, langpacks, priorities, update-motd
amzn2-core                                                                                                          | 3.7 kB  00:00:00     
Package docker-20.10.17-1.amzn2.x86_64 already installed and latest version
Nothing to do
[root@ip-172-31-91-4 ~]# 
[root@ip-172-31-91-4 ~]# systemctl start docker 
[root@ip-172-31-91-4 ~]# systemctl status  docker 
● docker.service - Docker Application Container Engine
   Loaded: loaded (/usr/lib/systemd/system/docker.service; enabled; vendor preset: disabled)
   Active: active (running) since Mon 2022-09-26 03:59:29 UTC; 1h 13min ago
     Docs: https://docs.docker.com
  Process: 3221 ExecStartPre=/usr/libexec/docker/docker-setup-runtimes.sh (code=exited, status=0/SUCCESS)
  Process: 3216 ExecStartPre=/bin/mkdir -p /run/docker (code=exited, status=0/SUCCESS)
 Main PID: 3231 (dockerd)
    Tasks: 8

```

### accessing and check docker installation 

```
root@ip-172-31-91-4 ~]# docker  version 
Client:
 Version:           20.10.17
 API version:       1.41
 Go version:        go1.18.3
 Git commit:        100c701
 Built:             Thu Jun 16 20:08:47 2022
 OS/Arch:           linux/amd64
 Context:           default
 Experimental:      true

Server:
 Engine:
  Version:          20.10.17
  API version:      1.41 (minimum version 1.12)
  Go version:       go1.18.3
  Git commit:       a89b842
  Built:            Thu Jun 16 20:09:24 2022
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.6.6
  GitCommit:        10c12954828e7c7c9b6e0ea9b0c02b01407d3ae1
 runc:
  Version:          1.1.3
  GitCommit:        1e7bb5b773162b57333d57f612fd72e3f8612d94
 docker-init:
  Version:          0.19.0
  GitCommit:        de40ad0

```

### to creating containers will need container app images 

<img src="img.png">

### container images from container registries 

<img src="reg.png">

###  images & containers 

<img src="contimg.png">






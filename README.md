# dk-cisco-26sep2022

## Training Plan

<img src="plan.png">

## Namespaces in linux kernel for container isolation 

<img src="ns.png">

### checking kernel context of a container 

### creating container 

```
[ashu@ip-172-31-91-4 ashu-images]$ docker  run -d --name c1 alpine ping fb.com 
8bc8ed394457f56ed4948daecb521a5ee32d13c2551a96fedcb9e07c68f9ae9b
[ashu@ip-172-31-91-4 ashu-images]$ docker  ps
CONTAINER ID   IMAGE     COMMAND         CREATED         STATUS         PORTS     NAMES
8bc8ed394457   alpine    "ping fb.com"   3 seconds ago   Up 2 seconds             c1
[ashu@ip-172-31-91-4 ashu-images]$ docker  

```

### getting container pid 

```
 docker  inspect  c1  --format='{{.State.Pid}}'
 17892
```

### From linux host -- with root access 

```
[root@ip-172-31-91-4 ~]# ps  -e  |   grep -i 17892
17892 ?        00:00:00 ping
[root@ip-172-31-91-4 ~]# ps -aux  |   grep -i 17892
root     17892  0.6  0.0   1600   896 ?        Ss   04:50   0:00 ping fb.com
root     18043  0.0  0.0 119404   940 pts/11   S+   04:50   0:00 grep --color=auto -i 17892
[root@ip-172-31-91-4 ~]# 
[root@ip-172-31-91-4 ~]# cd  /proc/17892
[root@ip-172-31-91-4 17892]# ls
arch_status  cmdline          exe      limits     mounts      oom_score      root       smaps_rollup  task
attr         comm             fd       loginuid   mountstats  oom_score_adj  sched      stack         timens_offsets
autogroup    coredump_filter  fdinfo   map_files  net         pagemap        schedstat  stat          timers
auxv         cpuset           gid_map  maps       ns          patch_state    sessionid  statm         timerslack_ns
cgroup       cwd              io       mem        numa_maps   personality    setgroups  status        uid_map
clear_refs   environ          latency  mountinfo  oom_adj     projid_map     smaps      syscall       wchan
[root@ip-172-31-91-4 17892]# ls  ns/
cgroup  ipc  mnt  net  pid  pid_for_children  time  time_for_children  user  uts
[root@ip-172-31-91-4 17892]# 

```

### Cgroups in Containers 

<img src="cg.png">

## Dockerfile for frontend webapp 

### Understanding webapp servers 

<img src="web.png">

### cloning git project 

```
git clone https://github.com/yenchiah/project-website-template.git
Cloning into 'project-website-template'...
remote: Enumerating objects: 1025, done.
remote: Total 1025 (delta 0), reused 0 (delta 0), pack-reused 1025
Receiving objects: 100% (1025/1025), 1.64 MiB | 14.59 MiB/s, done.
Resolving deltas: 100% (633/633), done.
[ashu@ip-172-31-91-4 webapps]$ ls
Dockerfile  project-website-template
[ashu@ip-172-31-91-4 webapps]$ 
```
### Dockerfile 

```
FROM nginx
LABEL name=ashutoshh
LABEL email=ashutoshh@Linux.com
ADD project-website-template  /usr/share/nginx/html/
# copy and add both can copy a directory also 
# if we don't write cmd/entrypoint then main image process will be 
# inherited  
```

### .dockerignore 

```
project-website-template/.git
project-website-template/.github
project-website-template/.gitignore
project-website-template/LICENSE
project-website-template/README.md

```

### lets build it 

```
[ashu@ip-172-31-91-4 webapps]$ ls -a
.  ..  Dockerfile  .dockerignore  project-website-template
[ashu@ip-172-31-91-4 webapps]$ 
[ashu@ip-172-31-91-4 webapps]$ docker build -t ashunginx:ciscov1 . 
Sending build context to Docker daemon   1.73MB
Step 1/4 : FROM nginx
 ---> 2d389e545974
Step 2/4 : LABEL name=ashutoshh
 ---> Running in 704a5a93e0d9
Removing intermediate container 704a5a93e0d9
 ---> daa934f7ce6a
Step 3/4 : LABEL email=ashutoshh@Linux.com
 ---> Running in 2728b7893382
Removing intermediate container 2728b7893382
 ---> 15a8616f2d68
Step 4/4 : ADD project-website-template  /usr/share/nginx/html/
 ---> f2863e6d3ed0
Successfully built f2863e6d3ed0
Successfully tagged ashunginx:ciscov1
```

### creating container 

```
ashu@ip-172-31-91-4 webapps]$ docker  run -d --name ashungc1  ashunginx:ciscov1  
89fca40be1e18c5e47028bb032c63ba333e9a50ef98a231006e9d737fac2f0d3
[ashu@ip-172-31-91-4 webapps]$ docker  ps
CONTAINER ID   IMAGE               COMMAND                  CREATED          STATUS          PORTS     NAMES
89fca40be1e1   ashunginx:ciscov1   "/docker-entrypoint.…"   4 seconds ago    Up 3 seconds    80/tcp    ashungc1
fe0d628e271f   9b35ca459926        "/docker-entrypoint.…"   38 seconds ago   Up 37 seconds   80/tcp    narasimhanginxcontainer
8bc8ed394457   alpine              "ping fb.com"            34 minutes ago   Up 30 minutes             c1
[ashu@ip-172-31-91-4 webapps]$ 
```

### adding cgroup layer in container 

```
docker  run -d --name ashungc1  --memory 100M --cpuset-cpus=0 --cpu-shares=256      ashunginx:ciscov1  
```



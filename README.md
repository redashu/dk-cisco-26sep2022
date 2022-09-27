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





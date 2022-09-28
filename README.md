# dk-cisco-26sep2022

## Training Plan

<img src="plan.png">

### volume to containers concept 

<img src="vol2cont.png">

## MYSQL db container example 

### MySQL volume Creation 

```
[ashu@ip-172-31-91-4 ~]$ docker  volume create ashudb-vol1 
ashudb-vol1
[ashu@ip-172-31-91-4 ~]$ docker  volume  ls
DRIVER    VOLUME NAME
local     ashudb-vol1
[ashu@ip-172-31-91-4 ~]$ docker  volume  inspect  ashudb-vol1 
[
    {
        "CreatedAt": "2022-09-28T04:35:56Z",
        "Driver": "local",
        "Labels": {},
        "Mountpoint": "/var/lib/docker/volumes/ashudb-vol1/_data",
        "Name": "ashudb-vol1",
        "Options": {},
        "Scope": "local"
    }
]
[ashu@ip-172-31-91-4 ~]$ 
```




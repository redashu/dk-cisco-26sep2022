# dk-cisco-26sep2022

## Training Plan

<img src="plan.png">

## Networking in k8s 

<img src="net.png">

### POd networking 

<img src="podnet.png">

### Calico Node agents and controllers creates distributed bridge 

<img src="calico.png">

### deploying sample pod and checking networking 

```
[ashu@ip-172-31-91-4 ~]$ kubectl  run  ashupod1  --image=alpine --command ping fb.com 
pod/ashupod1 created
[ashu@ip-172-31-91-4 ~]$ kubectl  get  po 
NAME              READY   STATUS        RESTARTS   AGE
ankurpod-123      1/1     Running       0          58s
ashupod1          1/1     Running       0          9s
bharti-123        1/1     Running       0          17s
narasimhapod1     1/1     Running       0          3s
priyankapod-123   1/1     Terminating   0          38s
sridhar           1/1     Running       0          3s
[ashu@ip-172-31-91-4 ~]$ kubectl  get  po -o wide
NAME            READY   STATUS    RESTARTS   AGE   IP                NODE            NOMINATED NODE   READINESS GATES
ankurpod-123    1/1     Running   0          82s   192.168.97.90     minion-node-2   <none>           <none>
ashupod1        1/1     Running   0          33s   192.168.174.221   minion-node-3   <none>           <none>
bharti-123      1/1     Running   0          41s   192.168.97.91     minion-node-2   <none>           <none>
mufimpod1       1/1     Running   0          10s   192.168.97.93     minion-node-2   <none>           <none>
narasimhapod1   1/1     Running   0          27s   192.168.174.222   minion-node-3   <none>           <none>
sridhar         1/1     Running   0          27s   192.168.97.92     minion-node-2   <none>           <none>
yashpod1        1/1     Running   0          12s   192.168.174.223   minion-node-3   <none>           <none>
[ashu@ip-172-31-91-4 ~]$ kubectl  describe  pod ashupod1
Name:             ashupod1
Namespace:        default
Priority:         0
Service Account:  default
Node:             minion-node-3/172.31.84.128
Start Time:       Thu, 29 Sep 2022 04:54:58 +0000
Labels:           run=ashupod1
Annotations:      cni.projectcalico.org/containerID: 01b8eeb3d3ae7400b27ea7748db2bcef47a7df15e8a07c03132e0ca4915c2134
                  cni.projectcalico.org/podIP: 192.168.174.221/32
                  cni.projectcalico.org/podIPs: 192.168.174.221/32
Status:           Running
IP:               192.168.174.221

```

### pods can communicate to each other by default 

```
[ashu@ip-172-31-91-4 ~]$ kubectl  get  po -o wide
NAME            READY   STATUS    RESTARTS   AGE     IP                NODE            NOMINATED NODE   READINESS GATES
ankurpod-123    1/1     Running   0          3m36s   192.168.97.90     minion-node-2   <none>           <none>
ashupod1        1/1     Running   0          2m47s   192.168.174.221   minion-node-3   <none>           <none>
balajin-pod1    1/1     Running   0          103s    192.168.138.75    minion-node-1   <none>           <none>
bharti-123      1/1     Running   0          2m55s   192.168.97.91     minion-node-2   <none>           <none>
mufimpod1       1/1     Running   0          2m24s   192.168.97.93     minion-node-2   <none>           <none>
narasimhapod1   1/1     Running   0          2m41s   192.168.174.222   minion-node-3   <none>           <none>
prampod         1/1     Running   0          2m6s    192.168.138.74    minion-node-1   <none>           <none>
priyankpod1     1/1     Running   0          106s    192.168.97.94     minion-node-2   <none>           <none>
sridhar         1/1     Running   0          2m41s   192.168.97.92     minion-node-2   <none>           <none>
steve-pod1      2/2     Running   0          56s     192.168.174.226   minion-node-3   <none>           <none>
suhas-123       1/1     Running   0          62s     192.168.174.225   minion-node-3   <none>           <none>
yashpod1        1/1     Running   0          2m26s   192.168.174.223   minion-node-3   <none>           <none>
[ashu@ip-172-31-91-4 ~]$ 
[ashu@ip-172-31-91-4 ~]$ 
[ashu@ip-172-31-91-4 ~]$ kubectl  exec -it  ashupod1  -- sh 
/ # ifconfig 
eth0      Link encap:Ethernet  HWaddr 3A:E0:61:3B:C2:02  
          inet addr:192.168.174.221  Bcast:0.0.0.0  Mask:255.255.255.255
          inet6 addr: fe80::38e0:61ff:fe3b:c202/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:8981  Metric:1
          RX packets:219 errors:0 dropped:0 overruns:0 frame:0
          TX packets:226 errors:0 dropped:1 overruns:0 carrier:0
          collisions:0 txqueuelen:0 
          RX bytes:22236 (21.7 KiB)  TX bytes:21142 (20.6 KiB)

lo        Link encap:Local Loopback  
          inet addr:127.0.0.1  Mask:255.0.0.0
          inet6 addr: ::1/128 Scope:Host
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:0 (0.0 B)  TX bytes:0 (0.0 B)

/ # ping  192.168.97.90
PING 192.168.97.90 (192.168.97.90): 56 data bytes
64 bytes from 192.168.97.90: seq=0 ttl=62 time=0.576 ms
64 bytes from 192.168.97.90: seq=1 ttl=62 time=1.041 ms
64 bytes from 192.168.97.90: seq=2 ttl=62 time=0.616 ms
^C
--- 192.168.97.90 ping statistics ---
3 packets transmitted, 3 packets received, 0% packet loss
round-trip min/avg/max = 0.576/0.744/1.041 ms
/ # ping  192.168.138.75
PING 192.168.138.75 (192.168.138.75): 56 data bytes
64 bytes from 192.168.138.75: seq=0 ttl=62 time=0.550 ms
64 bytes from 192.168.138.75: seq=1 ttl=62 time=0.502 ms
64 bytes from 192.168.138.75: seq=2 ttl=62 time=0.610 ms
^C
--- 192.168.138.75 ping statistics ---
3 packets transmitted, 3 packets received, 0% packet loss
round-trip min/avg/max = 0.502/0.554/0.610 ms
/ # 
[ashu@ip-172-31-91-4 ~]$ 

```

### calico installation on k8s link

[click_here](https://projectcalico.docs.tigera.io/getting-started/kubernetes/)




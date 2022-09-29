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




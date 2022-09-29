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


## K8s InterMediate Networking 

### External Lb to NOde--Internal-lB ---POd 

<img src="extlb.png">

### Internal LB can be created by Service Resources in k8s 

<img src="svc.png">

### Concept to labels to update Internal lB db with POdIP:port 

<img src="label.png">

## Practise Networking 

### creating webapp pod 

```
kubectl run ashuwebapp --image=docker.io/dockerashu/ciscoapp:v1  --port 80 --dry-run=client   -o yaml  >webapp.yaml 
```

### modified YAML 

```
apiVersion: v1
kind: Pod
metadata: # info about Resource like pod 
  creationTimestamp: null
  labels: # label of my pod 
    x1: helloashu # here x1 is key and helloashu is value 
  name: ashuwebapp # name of pod 
spec:
  containers:
  - image: docker.io/dockerashu/ciscoapp:v1
    name: ashuwebapp
    ports:
    - containerPort: 80
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}

```

### lets deploy it 

```
[ashu@ip-172-31-91-4 k8s-app-deploy]$ ls
ashu-pod1.yaml  auto.yaml  logs.txt  task1.yaml  webapp.yaml
[ashu@ip-172-31-91-4 k8s-app-deploy]$ kubectl apply -f webapp.yaml 
pod/ashuwebapp created
[ashu@ip-172-31-91-4 k8s-app-deploy]$ kubectl  get po 
NAME          READY   STATUS              RESTARTS   AGE
ashuwebapp    0/1     ContainerCreating   0          3s
sriwebapp     1/1     Running             0          2s
stevewebapp   1/1     Running             0          19s
[ashu@ip-172-31-91-4 k8s-app-deploy]$ kubectl  get po --show-labels
NAME              READY   STATUS    RESTARTS   AGE   LABELS
ashuwebapp        1/1     Running   0          7s    x1=helloashu
narasimhawebapp   1/1     Running   0          2s    x1=hellonarasimha
priyankawebapp    1/1     Running   0          3s    x1=hellopriyanka
sriwebapp         1/1     Running   0          6s    run=sriwebapp,x1=hellosri
stevewebapp       1/1     Running   0          23s   app-label=steve-webapp
```

### Introduction to Internal lB in k8s -- created by service resource under api v1 

<img src="stype.png">

### Service type with expose app options 

<img src="expose.png">

### NodePOrt vs LoadBalancer service 

<img src="nplb.png">

### creating nodeport service 

```
kubectl  create   service  nodeport  ashu-internal-lb1   --tcp 1234:80  --dry-run=client -o yaml >nodeport1.yaml 
```
###


<img src="np.png">

### creating serivces and checking EPs

```
[ashu@ip-172-31-91-4 k8s-app-deploy]$ kubectl apply -f nodeport1.yaml 
service/ashu-internal-lb1 created
[ashu@ip-172-31-91-4 k8s-app-deploy]$ kubectl   get  services
NAME                TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
ashu-internal-lb1   NodePort    10.99.248.229   <none>        1234:32298/TCP   11s
kubernetes          ClusterIP   10.96.0.1       <none>        443/TCP          22h
[ashu@ip-172-31-91-4 k8s-app-deploy]$ 
[ashu@ip-172-31-91-4 k8s-app-deploy]$ kubectl   get  endpoints 
NAME                 ENDPOINTS           AGE
ashu-internal-lb1    <none>              35s
kubernetes           172.31.87.27:6443   22h
sri-int-lb           <none>              22s
suhas-internal-lb1   <none>              15s
```


### updating selector section and reapply it 

```
apiVersion: v1
kind: Service
metadata:
  creationTimestamp: null
  labels: # label of service 
    app: ashu-internal-lb1
  name: ashu-internal-lb1 # name of service 
spec:
  ports:
  - name: 1234-80
    port: 1234 # internal LB port 
    protocol: TCP
    targetPort: 80 # target pod application port 
  selector: # pod finder using below given label 
    x1: helloashu # ashuapp pod label 
  type: NodePort # type of service 
status:
  loadBalancer: {}

```

### 

```
 kubectl  apply -f nodeport1.yaml 
 [ashu@ip-172-31-91-4 k8s-app-deploy]$ kubectl   get  svc  ashu-internal-lb1 
NAME                TYPE       CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
ashu-internal-lb1   NodePort   10.99.248.229   <none>        1234:32298/TCP   5m53s
[ashu@ip-172-31-91-4 k8s-app-deploy]$ 
[ashu@ip-172-31-91-4 k8s-app-deploy]$ kubectl  get  ep   ashu-internal-lb1 
NAME                ENDPOINTS          AGE
ashu-internal-lb1   192.168.97.95:80   6m
[ashu@ip-172-31-91-4 k8s-app-deploy]$ kubectl   get  po -o wide  |  grep ashu
ashuwebapp        1/1     Running   0          26m     192.168.97.95     minion-node-2   <none>           <none>
```

### External LB for single app 

<img src="extlbapp.png">

## 3 problems as of current section 

<img src="prob.png">

### Namespace in k8s 

<img src="ns.png">

### creating namespace 

```
ashu@ip-172-31-91-4 ~]$ kubectl   get  namespaces 
NAME              STATUS   AGE
default           Active   24h
kube-node-lease   Active   24h
kube-public       Active   24h
kube-system       Active   24h
[ashu@ip-172-31-91-4 ~]$ kubectl  create  namespace  ashu-apps 
namespace/ashu-apps created
[ashu@ip-172-31-91-4 ~]$ kubectl  get  ns
NAME              STATUS   AGE
ashu-apps         Active   4s
default           Active   24h
kube-node-lease   Active   24h
kube-public       Active   24h
kube-system       Active   24h
[ashu@ip-172-31-91-4 ~]$ 

```

### setting default namespace  & checking 

```
ashu@ip-172-31-91-4 ~]$ kubectl  config set-context --current --namespace  ashu-apps 
Context "kubernetes-admin@kubernetes" modified.
[ashu@ip-172-31-91-4 ~]$ 


[ashu@ip-172-31-91-4 ~]$ kubectl  config get-contexts 
CURRENT   NAME                          CLUSTER      AUTHINFO           NAMESPACE
*         kubernetes-admin@kubernetes   kubernetes   kubernetes-admin   ashu-apps
[ashu@ip-172-31-91-4 ~]$ 


```

### checking resources here 

```
[ashu@ip-172-31-91-4 ~]$ kubectl   get  pods
No resources found in ashu-apps namespace.
[ashu@ip-172-31-91-4 ~]$ 
[ashu@ip-172-31-91-4 ~]$ kubectl   get  svc
No resources found in ashu-apps namespace.
[ashu@ip-172-31-91-4 ~]$ 

```

### being super user in k8s we can access other namespace data also 

```
ashu@ip-172-31-91-4 ~]$ kubectl  get po -n default 
NAME              READY   STATUS             RESTARTS        AGE
ankurwebapp       1/1     Running            0               26m
balaj-webapp      1/1     Running            0               29m
mufimwebapp1      1/1     Running            0               15m
narasimhawebapp   1/1     Running            0               22m
pramodwebapp      0/1     CrashLoopBackOff   9 (2m43s ago)   24m
pramodwebapp1     1/1     Running            0               14m
priyankapod2      1/1     Running            0               29m
sridharpod        1/1     Running            0               21m
stevewebapp       1/1     Running            0               31m
suhas-webapp      1/1     Running            0               28m
yashwebapp        1/1     Running            0               22m
[ashu@ip-172-31-91-4 ~]$ kubectl  delete pod --all -n default 
pod "ankurwebapp" deleted
pod "balaj-webapp" deleted
pod "mufimwebapp1" deleted
pod "narasimhawebapp" deleted
pod "pramodwebapp" deleted
pod "pramodwebapp1" deleted
pod "priyankapod2" deleted

```



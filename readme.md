1- Install k8s cluster (minikube)
```
minikube start
```

2- Create a pod with the name Redis and with the image Redis.
```
$ kubectl create deploy redis --image redis
deployment.apps/redis created
```

3- Create a pod with the name Nginx and with the image nginx123. Use a pod-definition YAML file. And yes the image name is wrong!
<br>check nginx-pod.yaml file

4- What is the Nginx pod status?
<br>pod status => ErrImagePull

5- Change the Nginx pod image to Nginx check the status again
<br>pod status => Running 

6- How many ReplicaSets exist on the system?
<br>1
```
$ Kubectl get replicaset

NAME                          DESIRED   CURRENT   READY   AGE
redis-8464b6fbc9              1         1         1       35m
```

7- Create a ReplicaSet with
<br>
    name= replica-set-1
<br>
    image= busybox
<br>
    replicas= 3

8- Scale the ReplicaSet replica-set-1 to 5 PODs.
```
$ Kubectl scale --replicas=5 -f busybox-replicaset.yaml 
replicaset.apps/replica-set-1 scaled
```
9- How many PODs are READY in the replica-set-1?
<br>0

10- Delete any one of the 5 PODs then check How many PODs exist now?
<br>5
- Why are there still 5 PODs, even after you deleted one?
<br>because the replica-set still remains and it insures 5 replicas of the pod

11- How many Deployments and ReplicaSets exist on the system?
<br>
Deployments => 1
```
$ Kubectl get deployment

NAME               READY   UP-TO-DATE   AVAILABLE   AGE
redis              1/1     1            1           34m
```
Replicasets => 2

```
$ Kubectl get replicaset

NAME                          DESIRED   CURRENT   READY   AGE
redis-8464b6fbc9              1         1         1       35m
replica-set-1                 5         5         0       12m
```

12- Create a Deployment with
<br>
    name= deployment-1
<br>
    image= busybox
<br>
    replicas= 3

13- How many Deployments and ReplicaSets exist on the system now?
Deployments => 2
```
$ Kubectl get deployment
NAME               READY   UP-TO-DATE   AVAILABLE   AGE
deployment-1       0/3     3            0           11s
redis              1/1     1            1           46m
```
Replicasets => 3
```
$ Kubectl get replicaset
NAME                          DESIRED   CURRENT   READY   AGE
deployment-1-7fc8c7f7b9       3         3         0       54s
redis-8464b6fbc9              1         1         1       47m
replica-set-1                 5         5         0       24m
```

14- How many pods are ready with the deployment-1?
<br>0

15- Update deployment-1 image to Nginx then check the ready pods again
<br>3

16- Run kubectl describe deployment deployment-1 and check events, What is the deployment strategy used to upgrade the deployment-1? 

<br>RollingUpdate

17- Rollback the deployment-1 
```
$ kubectl rollout undo deploy deployment-1 --to-revision=1
deployment.apps/deployment-1 rolled back
```
- What is the used image with the deployment-1?
<br>busybox

18- How many Namespaces exist on the system?
<br>Namespaces => 4
```
$ Kubectl get namespaces
NAME              STATUS   AGE
default           Active   17h
kube-node-lease   Active   17h
kube-public       Active   17h
kube-system       Active   17h
```

19- How many pods exist in the kube-system namespace?
<br>kube-system pods => 7
```
$ Kubectl get pods -n kube-system
NAME                               READY   STATUS    RESTARTS        AGE
coredns-64897985d-ll7pd            1/1     Running   1 (3h43m ago)   17h
etcd-minikube                      1/1     Running   1 (3h43m ago)   17h
kube-apiserver-minikube            1/1     Running   1 (3h43m ago)   17h
kube-controller-manager-minikube   1/1     Running   1 (3h43m ago)   17h
kube-proxy-rqm49                   1/1     Running   1 (3h43m ago)   17h
kube-scheduler-minikube            1/1     Running   1 (3h43m ago)   17h
storage-provisioner                1/1     Running   3 (3h43m ago)   17h
```

20- Create a deployment with
<br>
     Name: beta
<br>
     Image: Redis
<br>
     Replicas: 2
<br>
     Namespace: finance
<br>
     Resources Requests:
<br>
       CPU: .5 vcpu
<br>
       Mem: 1G
<br>
    Resources Limits:
<br>
      CPU: 1 vcpu
<br>
      Mem: 2G

<br>check file redis-deployment.yaml

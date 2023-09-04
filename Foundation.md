# OPENSHIFT

- Container Orchestartion Platform based on Kubernetes.
- Can do Operations & Development
- Supports most of the Coding Language(Java, python,Ruby, PHP, .NET)
- OCP control plane only can be deployed on RHCOS. (RHCOS is only for OCP deployment)
- OCP workloads can be deployed on RHEL or RHCOS.
  
  ![Alt text](Images/Image1.PNG)
  

## ARCHITECTURE

### Nodes

- OpenShift only runs on RHEL & RHCOS.
- OpenShift has 2 types of nodes.
  - Masters (End-users applications runs)
      - Orchestarte activities on worker node.
      - Maintain the state within OpenShift environment
      - Three masters for HA
  - Workers (Manages the Cluster)

### etcd

- It stores all the data related to the cluster in a key:value pair.
- 

### Containers

- Containers are application run time.
- Images are Application "binary".
- Worker nodes can run many containers.

### Pod

- Pod is the smallest most basic component in the cluster.
- One or more containers can run on a single pod. If multiple containers are running then they share the same storage and network.
- OCP schedules & runs all containers in Pod on same node.

### Service

- It is used to expose the application running on the pods to the outside world. 
- Defines logical set of pods.
- Provides permanent IP address and hostname for the Applications.


## Deployment Strategies

Deployment strategy we can use RollingUpdate & Recreate.

For selecting deployment strategy we need to set .spec.strategy.type on the Deployment object yaml.
- **RollingUpdate**: 
  - This is used when you want to graudally roll out without downtime.
  - Here new pods are created along with the older ones and thet graduallly replaced one by one.
- **Recreate**: 
  - This is a simplest strategy when you want to recreate all the pods at once.
  - All the pods are terminated and new pods generates. This will give a brief downtime.

- When we do any changes in the deployment, everytime OpenShift rollosout the updates. So to prevent this we can pause the rollout and once all the changes made we can resume the rollout.

```
oc rollout pause deployment/<deployment-name>
oc set image deployment/<deployment-name> <image>
oc  set env deployment/<deployment-name> NGINX_LOG_TO_VOLUME=1
oc set probe deployment/<deployment-name> --readiness --get-url http://:8080
oc rollout resume deployment/<deployment-name>
```

*In the above commands we paused the deployment rollout then updated the deployment with new image, set env variable and add probe. After that started the rollout. So the deployment will rollout once.*

*If ew won't pause, for every updates there would be a rollout.*

### Rollout

For rolling back to the previous version we can use 
```
oc rollout undo deployment/<dep-name>
```
By default it will bring the application to previous version. But for a particular version use history and from there use the available revision.
```
oc rollout history deployment/<dep-name>
oc rollout undo deployment/<dep-name> --to-revision 1

```

- When we run oc rollout history it gives a column with CHANGE_CAUSE. By default it is blank. We can mention it by using oc annotation.

```
oc annotate deployment/myapp2 kubernetes.io/change-cause="Image updated to 1-86"
```

## Image Stream










# Questions

1. **Disconnected upgradation process?**

    - First thing we need is a disonnnected registry where we will store the required images. The registry might be quay or docker or any other.
   - Mirror all the required images to the registry. We can use skopeo or podman for this.
   - Once all the images are present we need to create a tarball of the images and send it to the master node.

2. **What is the purpose of the OpenShift Operator Framework?**

   - OpenShift Operators automate the creation, configuration, and management  of Kubernetes-native applications. Operator provides automation at every level.

3. **How do you troubleshoot application *performance* issues in an OpenShift cluster?**

   - Troubleshooting application can be done by using Monitoring and alerting. Tools like Prometheous and grafana can be used. We can set performance metrics and alerting on deviations on the metrics.
   - Then we need to identify whether the problem is related to application or to infrastructure or related to both.
   - Check resource usages including CPU,memory and storage. Identify any application or pod ming excessive resources.For this we can use **oc describe pod/podname** or **oc logs podname**
   - Exmine the application or container logs for identifying any errors or any issue.
   - If resource issue then we can scale the application horizontally or vertically. 
   - Check if any other application utilizing the resources. If so you can move the pod to other node.
   - Check node resource utilization by using **oc adm top nodes**
   - Check the OS and kernel are free of any issue.

4. **Describe the process for scaling an application in OpenShift, both horizontally and vertically?**

   - Horizontally we can use 
    **oc scale deployment deployment-name --replicas=2**

   - Vertically we can use
    **oc get pods**
    **oc edit pod/podname**

  Here we can set the resource limit and request

   - Basically scaling horizontally is scaling out that means increaseing the number of replicas.
   - Scaling vertically means scaling the resource request and resource limit in the pod.

5. Purpose of HPA in OpenShift.

   - Horizontal Pod autoscaller basically used when we need to scale out the pods. When the load request is high we need to automatically scale the pods. So for that we use HPA.
   - We can use **oc autoscale deployment <deployment-name> --cpu-percent=50 --min=2 --max=10**

6. What is a Rolling Deployment strategy in OpenShift, and when would you use it?

    - There are mainly 2 deployment strategy(deployment rollout). Blue greeen and canary. Other than these there is also recreate and Rolling update
    - In blue green strategy we mainly devide our application environment into 2 parts where we redirect our application traffic using load balancer to one part. Then we deploy our new application update to the other half.
    - In canary deployment 
    - In Recreate strategy what we do is, we kill all the pods and another set of pods will be deployed. But there will be some downtime.
    - In rolling update we will replace pods one by one with the latest version.
    - 
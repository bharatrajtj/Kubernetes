## Understanding Kubernetes Deployments

In Kubernetes, orchestrating containers involves a structured hierarchy. At the core, containers find their home within a pod, and these pods, in turn, are organized within a replica set. The orchestration extends further, with the replica set residing within a Deployment.

### The Deployment Cascade

Launching a Deployment triggers the creation of a replica set, within which pods are instantiated. The replica set serves as a Kubernetes controller, implementing automatic healing for pods. It diligently ensures that the desired and actual states align.

### Deployment Versus Replica Set

While both Deployment and replica set function as Kubernetes controllers, Deployment offers additional capabilities. It facilitates seamless updates to container versions within pods. Unlike replica sets, which recreate pods with initial configurations, Deployment empowers users to upgrade or roll back container versions.

### Upgrading with Deployment

When a container version update is in motion, Deployment crafts a new replica set. This set houses the updated version, ensuring a smooth transition. The default strategy employed by Deployment is the Rolling Update strategy. Imagine five pods within a replica set—the rollout involves creating a new replica set, terminating some pods from the current set, introducing pods with the latest configuration, and eventually phasing out the remaining pods in favor of the new replica set.

### Undoing Changes

Deployment's versatility shines through when changes need to be undone. Whether it's a version rollback or a step back in configuration, Deployment supports an undo operation. Executing this operation reverts to a previous replica set, effectively restoring the pods to their earlier state.

In essence, Kubernetes Deployments offer a powerful and flexible approach to managing containerized applications, ensuring a robust and controlled deployment lifecycle.



The following is a deployment manifest


```YAML
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
```
The execution of the deployment manifest will yield 1 deployment with 3 replica set and 3 pods in it
![image](https://github.com/bharatrajtj/Kubernetes/assets/65009556/5d1581ee-8034-46e5-b8e5-ff4fbb80a468)

When one pod is deleted, the replica set notice there is a change in the actual state with respect to desired state and create a pod to maintain balance between both states.
![image](https://github.com/bharatrajtj/Kubernetes/assets/65009556/b6030353-fac0-40c8-9e21-6d30c9bd8ed9)

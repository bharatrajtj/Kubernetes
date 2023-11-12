# Kubernetes Service

## Problem Statement

In Kubernetes, when a pod goes down and a new pod is deployed by the replica set, the new pod gets a different IP address. This poses a challenge for users who were initially provided with the IP address of the first pod, as they are not updated with the new IP address of the replacement pod.

## Solution:

To address this issue, Kubernetes introduced the concept of a **service**. The service is created on top of the deployment and acts as a load balancer with the help of kube proxy. Instead of users accessing pods directly using their IP addresses, they now use the IP address of the service. The service forwards requests from users by distributing traffic among the available pods.

## Service and Labels

Each pod is created with a label, and services use these labels to forward traffic to the desired pod. Even if a pod fails and a new one is created with a new IP address, the label remains the same. The service uses **selectors** to identify pods based on these labels, ensuring seamless traffic forwarding. This concept is known as **service discovery**.

## Advantages of Using Services

Services offer three key advantages:

1. **Load Balancing**: Traffic is distributed among available pods, ensuring efficient resource utilization.

2. **Service Discovery**: The use of labels and selectors enables automatic discovery and routing of traffic to the correct pods.

3. **Exposure**: Services provide different types of exposure, such as Cluster IP, Load Balancer, and Node Port.

## Hands-On Experience
### Step 1: Build Image
Git clone the following git url to your server.
```
git clone https://github.com/iam-veeramalla/Docker-Zero-to-Hero.git
```
Navigate to examples direcctory
```
cd examples
```
Navigate to python-web-app
```
cd python-web-app
```

![image](https://github.com/bharatrajtj/Kubernetes/assets/65009556/d4427e86-e7e7-4d50-9a7d-41f95107154a)

Build the docker image using the dockerfile available in the directory
```
docker build -t python:v1
```
![image](https://github.com/bharatrajtj/Kubernetes/assets/65009556/92921613-f460-4d02-9fae-df05c72fc2ed)


### Step 2: Deployment

Use the following YAML file to create a deployment:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: pythonapp
  labels:
    app: demo
spec:
  replicas: 3
  selector:
    matchLabels:
      app: demo
  template:
    metadata:
      labels:
        app: demo
    spec:
      containers:
      - name: application
        image: bharatdevops/images:v1
        ports:
        - containerPort: 8000
```
Apply the deployment using:
```
kubectl apply -f deployment.yml
```
Check the deployment list:
```
kubectl get deploy
```
![image](https://github.com/bharatrajtj/Kubernetes/assets/65009556/56bf6a41-29ab-4979-8eeb-7e08177da4d8)

### Step 3: Pod IP Address
Get the pod IP addresses:
```
kubectl get pods -o wide
```
Delete the pods, now the replica set will see that there is a difference in required and current state and create two new pods.
Notice the pod IP address is different this time. The users using this pods will not be updated with the new pod IP address and therefore they will not be able to access the application even when it is running.
![image](https://github.com/bharatrajtj/Kubernetes/assets/65009556/89f6ae94-c597-4703-9490-868de7849569)

### Step 4: Create Service
Use the following YAML file to create a NodePort service:
```
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  type: NodePort
  selector:
    app: demo
  ports:
    - port: 80
      targetPort: 8000
      nodePort: 30007
```
(cluster ip is assigned to a service. the service can be accesed by pods or other service through port. that is the traffic receiveed by cluster ip will be forward to pods target port via port.
port is for the service within the cluster.
targetPort is for the pods associated with the service.
nodePort is used for accessing the service externally from the worker nodes.)

Apply the service using:
```
kubectl apply -f service.yml
```
![image](https://github.com/bharatrajtj/Kubernetes/assets/65009556/93dfb79b-4418-495e-8b92-d18305e7364a)

Notice the creation of a NodePort service.

### Step 5: Access the Application
Now, access the application using the Minikube IP address mapped to the NodePort.

NodePort: [Minikube-IP]:30007
This can be accessed within the organization's network.
![image](https://github.com/bharatrajtj/Kubernetes/assets/65009556/adcc86c7-2eea-4323-b9d7-f68c59e12b43)
This can not be accessed in website out of the organizations network.
![image](https://github.com/bharatrajtj/Kubernetes/assets/65009556/9ed3329a-9fa5-4029-8cc5-4d48d3335547)

### Step 6: Cluster IP
Cluster IP is the default service assigned by Kubernetes. Access the application within the Minikube cluster
![image](https://github.com/bharatrajtj/Kubernetes/assets/65009556/677dc3b1-4c51-45f9-a162-e137326ee6d7)

This cannot be accessed via website
![image](https://github.com/bharatrajtj/Kubernetes/assets/65009556/6d4c2baf-b18e-4294-8176-5d7866e664fa)




## Kubernetes Ingress: A Solution to Cost-Efficiently Expose Services

In the pre-Kubernetes era, virtual machines (VMs) were the go-to solution for running applications, utilizing enterprise-level load balancing with features like sticky sessions, ratio-based, white-listing, black-listing, path-based, and domain-based load balancing. However, Kubernetes (K8s) introduced a paradigm shift.

### Challenges with Load Balancers in K8s

The default load balancing mechanism in K8s is round-robin, which may not cover all the use cases offered by traditional enterprise load balancers. Moreover, exposing services to the external world using load balancers results in static and public IP addresses, incurring charges from cloud providers. With an increasing number of services, the cost becomes a significant concern.

### Enter Ingress: A Cost-Effective Alternative

K8s addressed this issue by introducing the concept of Ingress. Instead of relying on individual load balancer IP addresses, Ingress provides a scalable and cost-effective solution. Ingress controllers, developed by load balancer providers, offer diverse load balancing options such as path-based and domain-based.

### How Ingress Works in K8s

1. **Ingress Resource Deployment**: Users deploy an Ingress resource in the K8s cluster, specifying rules for routing traffic.

2. **Ingress Controller Deployment**: Load balancer providers offer Ingress controllers, which are deployed in the K8s cluster using Helm charts or YAML manifests. These controllers monitor the Ingress resources.

3. **Efficient Resource Utilization**: Unlike the one-to-one relationship between services and load balancer IPs, a single Ingress can be used by multiple K8s services, optimizing resource utilization.

### Code Showcase: Sample Ingress Resource

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: ingress-demo
spec:
  rules:
  - host: "bharatdevops.com"
    http:
      paths:
      - pathType: Prefix
        path: "/demo"
        backend:
          service:
            name: my-service
            port:
              number: 80
```
In this example, the Ingress resource defines rules for routing traffic based on the host and path. The external endpoint "bharatdevops.com/demo" is directed to the "my-service" within the K8s cluster.

Apply the above ingress.yml file
```
kubectl apply -f ingress.yml
```
![image](https://github.com/bharatrajtj/Kubernetes/assets/65009556/19751d56-66b9-41de-afa2-7c0849ad9aeb)

Get ingress status
```
kubectl get ingress
```
![image](https://github.com/bharatrajtj/Kubernetes/assets/65009556/817a48a7-e604-4ee0-b158-77a784464f16)

Notice IP address has not been alloted for the ingress resource, this is because we have not deployed ingress controller into the cluster. Ingress resource is of no use, if ingress controller is not deployed.

Follow the following information to deploy NGINX ingress controller in minikube cluster.
![image](https://github.com/bharatrajtj/Kubernetes/assets/65009556/0e83b454-4ffe-43ac-8e79-a35a374828e7)

![image](https://github.com/bharatrajtj/Kubernetes/assets/65009556/f4fe5f83-8214-4ec6-8122-a682b6bfc4d6)

Make sure ingress controller is running. (ingress controller is also a pod)

![image](https://github.com/bharatrajtj/Kubernetes/assets/65009556/734a1441-7092-4aca-9e07-652a2a162807)


![image](https://github.com/bharatrajtj/Kubernetes/assets/65009556/c8f31784-a32e-4dab-9fce-64478440f121)

If you curl immediately with the host name you will get an error.
![image](https://github.com/bharatrajtj/Kubernetes/assets/65009556/70b3a899-f7f6-4a48-ad8e-d2abcdde8cd0)

This is because the host name is not mapped to IP address, this host name exist only inside the minikube cluster and it is not registered domain name.

Map the IP address to host in
```
vim /etc/hosts
```
![image](https://github.com/bharatrajtj/Kubernetes/assets/65009556/57869a83-1cee-4b32-8e5a-3becc326360a)

Then try to connect using the host name


![image](https://github.com/bharatrajtj/Kubernetes/assets/65009556/f2a5dd9b-2cfa-4fb8-b513-ab57a9d74680)

### Service and ingress
Service (Svc): Serves as a stable internal (cluster) IP, facilitating communication between pods within the cluster. It acts as an internal mechanism for load balancing and communication.

Ingress: Operates as a stable external endpoint, enabling external traffic to reach services within the cluster. Ingress excels at managing external access, providing flexibility through rules for routing traffic based on host and path.


### Conclusion

Ingress in Kubernetes serves as a bridge between internal services and external access, overcoming the limitations of traditional load balancing approaches. It not only optimizes costs but also provides a flexible and scalable solution for managing external access to services in a Kubernetes environment.

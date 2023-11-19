# Kubernetes ConfigMaps and Secrets:

Kubernetes, with its powerful orchestration capabilities, relies on resources like ConfigMaps and Secrets to streamline the management of configuration data within a cluster. Let's delve into these essential components and explore their nuances.

## ConfigMaps: Unlocking Configurations

ConfigMaps serve as the go-to solution for storing configuration details, be it key-value pairs or entire files. Once deployed in a Kubernetes cluster, ConfigMaps can be effortlessly mounted into any pod, allowing pods to retrieve crucial information stored as environment variables or files. This flexibility empowers developers to access and utilize configuration data as needed, without the hassle of pod restarts.

## Secrets: Safeguarding Sensitive Data

In the realm of sensitive information, Secrets take the spotlight. The impetus behind Secrets is rooted in the realization that data saved in ConfigMaps is stored in etcd as an object. This raises concerns about security, especially in the face of potential hacker exploits gaining access to etcd.

To fortify against such threats, Kubernetes encrypts the information entered into Secrets at rest. This encryption occurs before the data is transported to etcd, providing an additional layer of security. Kubernetes even allows users to implement custom encryption for heightened protection. Consequently, even if a hacker breaches etcd, accessing Secrets becomes a formidable challenge without the requisite decryption keys.

## Best Practices for Secrets Implementation

Implementing Secrets necessitates a thoughtful approach to security. Strong Role-Based Access Control (RBAC) should be a cornerstone of your strategy. Not every user should be granted access to Secrets resources, ensuring that sensitive information remains tightly guarded.

## Overcoming ConfigMap Limitations

One notable limitation of ConfigMaps is the inability to update or change environment variables once they are loaded into a container. To circumvent this constraint, volume mounts come to the rescue. By saving ConfigMap data as files instead of environment variables, developers gain the flexibility to modify configurations dynamically, enhancing the adaptability of Kubernetes deployments.

## Hands-on

This is a simple ConfigMap file with db-port as the Key and 8000 as the value.

![image](https://github.com/bharatrajtj/Kubernetes/assets/65009556/e1a59015-945c-44b1-b53c-b4d7eea1c6e1)


Apply the configMap and check whether it is deployed.

![image](https://github.com/bharatrajtj/Kubernetes/assets/65009556/006578b6-6e30-41ca-9c44-7f6cf251f9af)

Update the deployment file to add ENV

![image](https://github.com/bharatrajtj/Kubernetes/assets/65009556/4921605e-0a76-449b-b36b-0fe9f0153c88)


Get the pod name and exec into the pod, search for ENV

![image](https://github.com/bharatrajtj/Kubernetes/assets/65009556/4ed7d1c3-297d-4b7d-baf9-56578fbf36b4)


Now, change the data value in ConfigMap

![image](https://github.com/bharatrajtj/Kubernetes/assets/65009556/f62e0113-2de1-45b9-85ff-39970afd7955)

This change will not be reflected inside the pods as env will not be updated. So, we create a volume and mount this volume to the pod

![image](https://github.com/bharatrajtj/Kubernetes/assets/65009556/7527235c-06e8-4426-89dd-138114f6be08)

![image](https://github.com/bharatrajtj/Kubernetes/assets/65009556/d4896cd5-03f9-47a7-b9b9-47d990c7bd6d)




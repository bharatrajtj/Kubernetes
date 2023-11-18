# Extending Kubernetes: Custom Resources and Controllers

## Introduction

In the dynamic world of Kubernetes, introducing new resources involves a choreography between two main actors: the DevOps Engineer and the User. This process relies on Custom Resource Definitions (CRDs) and custom controllers, orchestrating a seamless integration of bespoke resources into a Kubernetes cluster.

## Key Concepts

### Custom Resource Definition (CRD)

- **Definition:**
  - CRD is the blueprint, a YAML file that introduces a new API type to Kubernetes.

- **Extension of API Server:**
  - Kubernetes extends its API server to accommodate the new custom resource, delegating the resource's logic to custom controllers.

### Custom Controller

- **Deployment:**
  - Deployed in the Kubernetes cluster, the custom controller actively monitors and manages the behavior of the introduced custom resource.

- **Language Preference:**
  - While Golang is a preferred language for writing custom controllers, Kubernetes also supports controllers written in Python and Java.

## Deployment Steps

1. **Deploy CRD in Kubernetes Cluster (DevOps Engineer):**
   - Execute `kubectl apply -f custom_resource_definition.yaml` to make Kubernetes aware of the new custom resource type.

2. **Deploy Custom Resource Controller (DevOps Engineer):**
   - Develop and deploy the custom controller to actively watch and manage instances of the custom resource.

3. **Deploy Custom Resource in Kubernetes (User):**
   - Users create instances of the custom resource using YAML files and deploy them using `kubectl apply -f custom_resource_instance.yaml`.

## Conclusion

Understanding the interplay between CRDs, custom controllers, DevOps Engineers, and Users is key to extending the capabilities of Kubernetes. This structured approach ensures a smooth integration of custom resources, offering flexibility and customization within the Kubernetes ecosystem.

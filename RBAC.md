# Kubernetes RBAC and Access Management Portfolio

## Overview

In Kubernetes (K8s), Role-Based Access Control (RBAC) is a critical component for managing access and permissions within a cluster. RBAC enables the fine-grained control of who can perform specific actions on resources, enhancing security and governance.

## RBAC Management Components

### 1. Service Accounts/Users

Service accounts and users are entities in Kubernetes that require access to cluster resources. They serve as the identity for pods and users interacting with the cluster.

### 2. Roles/Cluster Roles

Roles and Cluster Roles define access levels and permissions within Kubernetes. Roles are created for namespace-level access, while Cluster Roles extend these permissions across the entire cluster.

### 3. Role Binding/Cluster Role Bindings

Role binding and Cluster Role Bindings associate users or service accounts with specific roles or cluster roles, defining the scope of their access within the cluster.

## User Management in Kubernetes

Kubernetes delegates user management to external identity providers. The API server acts as an OAuth server, ensuring secure authentication and authorization. In Amazon EKS (Elastic Kubernetes Service), IAM users can seamlessly integrate with Kubernetes as K8s users through IAM OAuth providers.

## Default Service Account

When a pod is created in Kubernetes, a default service account is automatically generated. This service account facilitates communication between pods and the API server, ensuring secure interactions.

## Role Definition

A Role in Kubernetes outlines the permissions granted and the resources accessible. Similar to IAM policies, a Role can be created at either the cluster level (Cluster Role) or the namespace level (Role), providing flexibility in access control.

## Role Binding

The process of associating a role with a user or service account is known as role binding. This step ensures that the defined permissions are applied to the respective entities within the cluster.

## Conclusion

Understanding and effectively implementing RBAC in Kubernetes is crucial for maintaining a secure and well-governed cluster environment.

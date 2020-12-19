---
title: Install Jenkins with Jenkins Operator
description: This tutorial explains how to Install Jenkins with Jenkins Operator
---
### Install Jenkins with Jenkins Operator
The Jenkins Operator is a Kubernetes native Operator which manages operations for Jenkins on Kubernetes. It was built with immutability and declarative configuration as code in mind. The Jenkins Operator is easy to install with just a few manifest and allows users to configure and manage Jenkins on Kubernetes

### Requirements
To run Jenkins Operator, you will need:

- Access to a Kubernetes cluster.
- kubectl version 1.11+

### Configure Custom Resource Definition
The Custom Resource Definition (CRD) API has been introduced to Kubernetes in v1.7 and it enables users to add custom APIs to their Kubernetes cluster which can be used like any other native Kubernetes objects. Defining a CRD object creates a new custom resource with a name and schema that you specify. The Kubernetes API serves and handles the storage of your custom resource.

```execute
kubectl apply -f https://raw.githubusercontent.com/jenkinsci/kubernetes-operator/master/deploy/crds/jenkins_v1alpha2_jenkins_crd.yaml
```

### Deploy Jenkins Operator

Using YAML’s
- Apply Service Account and RBAC roles:

```execute
kubectl apply -f https://raw.githubusercontent.com/jenkinsci/kubernetes-operator/master/deploy/service_account.yaml
kubectl apply -f https://raw.githubusercontent.com/jenkinsci/kubernetes-operator/master/deploy/role.yaml
kubectl apply -f https://raw.githubusercontent.com/jenkinsci/kubernetes-operator/master/deploy/role_binding.yaml
kubectl apply -f https://raw.githubusercontent.com/jenkinsci/kubernetes-operator/master/deploy/operator.yaml
```

Using below command Watch Jenkins Operator instance being created:

```execute
kubectl get pods
```

Now Jenkins Operator should be up and running in the default namespace.

You will see output similar below:

```
NAME                                READY   STATUS    RESTARTS   AGE
jenkins-operator-5f7979f97b-kdj89   1/1     Running   0          54s
```
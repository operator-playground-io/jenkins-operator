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

### Deploy Jenkins Operator

Using YAML’s
```execute
kubectl create -f https://operatorhub.io/install/jenkins-operator.yaml
```

Using below command Watch Jenkins Operator instance being created:

```execute
kubectl get pods -n my-jenkins-operator
```

Now Jenkins Operator should be up and running in the `my-jenkins-operator` namespace.

You will see output similar below:

```
NAME                                READY   STATUS    RESTARTS   AGE
jenkins-operator-5f7979f97b-kdj89   1/1     Running   0          54s
```
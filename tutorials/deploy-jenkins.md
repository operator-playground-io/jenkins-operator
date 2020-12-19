---
title: jenkins Operator Tutorial to deploy jenkins
description: This tutorial explains how to deploy jenkins
---

### Deploy Jenkins

Once Jenkins Operator is up and running let’s use below example:

```
apiVersion: jenkins.io/v1alpha2
kind: Jenkins
metadata:
  name: example
spec:
  master:
   image: jenkins/jenkins:lts
  seedJobs:
  - id: jenkins-operator
    targets: "cicd/jobs/*.jenkins"
    description: "Jenkins Operator repository"
    repositoryBranch: master
    repositoryUrl: https://github.com/jenkinsci/kubernetes-operator.git
```

**Step 1:** Apply Jenkins Custom Resource:

```execute
kubectl apply -f https://raw.githubusercontent.com/jenkinsci/kubernetes-operator/master/deploy/crds/jenkins_v1alpha2_jenkins_cr.yaml
```

Sample output:

```
jenkins.jenkins.io/example created
```

**Step 3:** Watch the Jenkins instance being created:.

```execute
kubectl get pods -n default
```

You will see output similar like below:

```
NAME                                READY   STATUS    RESTARTS   AGE
jenkins-operator-5f7979f97b-kdj89   1/1     Running   0          3m4s
```

It may take up to a few minutes until all the resources are created and the cluster is ready for use.

```
NAME                                READY   STATUS    RESTARTS   AGE
jenkins-example                     1/1     Running   0          54s
jenkins-operator-5f7979f97b-kdj89   1/1     Running   0          5m25s
```

**Note - Please wait till `Status` will be `Running` and `READY` should be 1/1 , and then proceed further.**

### Get Jenkins credentials

```execute
SECRET_NAME=jenkins-operator-credentials-example
kubectl -n default get secret $SECRET_NAME -o 'jsonpath={.data.user}' | base64 -d
kubectl -n default get secret $SECRET_NAME -o 'jsonpath={.data.password}' | base64 -d
```

### Connect to Jenkins (actual Kubernetes cluster)

* Execute below command to use NodePort:

```execute
kubectl port-forward jenkins-example 8080:8080
```

You will see output similar below:

```
Forwarding from 127.0.0.1:8080 -> 8080
Forwarding from [::1]:8080 -> 8080
```

Click on the <a href="https://##DNS.ip##:8080" target="_blank">https://##DNS.ip##:8080</a> to access Jenkins Dashboard

Now, you can prepare your own pipelines and update spec.seedJobs section in your Jenkins manifest.
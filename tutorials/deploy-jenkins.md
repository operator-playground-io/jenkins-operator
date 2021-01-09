---
title: jenkins Operator Tutorial to deploy jenkins
description: This tutorial explains how to deploy jenkins
---

### Deploy Jenkins

Once Jenkins Operator is up and running let’s use below example :

**Step 1:** Create `jenkins-instance` file:

```execute
cat <<'EOF' > jenkins-instance.yaml
apiVersion: jenkins.io/v1alpha2
kind: Jenkins
metadata:
  name: example
spec:
  master:
    containers:
    - name: jenkins-master
      image: jenkins/jenkins:lts
      imagePullPolicy: Always
      livenessProbe:
        failureThreshold: 12
        httpGet:
          path: /login
          port: http
          scheme: HTTP
        initialDelaySeconds: 80
        periodSeconds: 10
        successThreshold: 1
        timeoutSeconds: 5
      readinessProbe:
        failureThreshold: 3
        httpGet:
          path: /login
          port: http
          scheme: HTTP
        initialDelaySeconds: 30
        periodSeconds: 10
        successThreshold: 1
        timeoutSeconds: 1
      resources:
        limits:
          cpu: 1500m
          memory: 3Gi
        requests:
          cpu: "1"
          memory: 500Mi
  seedJobs:
  - id: jenkins-operator
    targets: "cicd/jobs/*.jenkins"
    description: "Jenkins Operator repository"
    repositoryBranch: master
    repositoryUrl: https://github.com/jenkinsci/kubernetes-operator.git
EOF
```

**Step 1:** Apply Jenkins Custom Resource:

```execute
kubectl create -f jenkins_instance.yaml -n my-jenkins-operator
```

Sample output:

```
jenkins.jenkins.io/example created
```

**Step 3:** Watch the Jenkins instance being created:.

```execute
kubectl get pods -n my-jenkins-operator
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
kubectl -n my-jenkins-operator get secret $SECRET_NAME -o 'jsonpath={.data.user}' | base64 -d
kubectl -n my-jenkins-operator get secret $SECRET_NAME -o 'jsonpath={.data.password}' | base64 -d
```

### Connect to Jenkins (actual Kubernetes cluster)

To access etcd cluster externally, lets first update service to use NodePort:

* Execute below command to use NodePort:
```execute
kubectl get service jenkins-operator-http-example --output yaml -n my-jenkins-operator > /tmp/my-jenkins.yaml
sed -i "s/type: .*/type: NodePort/g" /tmp/my-jenkins.yaml
kubectl patch svc jenkins-operator-http-example -p "$(cat /tmp/my-jenkins.yaml)" --namespace my-jenkins-operator
```

* Execute below command to update NodePort to 32379:
```execute
kubectl get service jenkins-operator-http-example --output yaml -n my-jenkins-operator > /tmp/my-jenkins.yaml
sed -i "s/nodePort: .*/nodePort: 32379/g" /tmp/my-jenkins.yaml
kubectl patch svc jenkins-operator-http-example -p "$(cat /tmp/my-jenkins.yaml)" --namespace my-jenkins-operator
```

Click on the <a href="http://##DNS.ip##:32379" target="_blank">http://##DNS.ip##:32379</a> to access Jenkins Dashboard

Now, you can prepare your own pipelines and update spec.seedJobs section in your Jenkins manifest.

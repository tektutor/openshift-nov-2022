# Day 7

## TekTon Overview
- TekTon a CI/CD Framework that works within Kubernetes/OpenShift
- in other words, it's a knative Frameworks that supports CI/CD
- We can install TekTon CI/CD Framework as an Operator or using manifest files(YAML files)
- it is an opensource project but backed by Red Hat
- it is nicely integrated in Kubernetes and OpenShift

## TekTon Operator
- will install many new Custom Resources like Task, TaskRun, Pipeline, PipelineRun, Workspace, etc.,

## What is a Task?
- Task is a series a steps executed one after the other in a top to bottom order
- Each Task is a Pod
- Task has one or more Steps
- Each Step is a Container that runs within the Task Pod

## What is a TaskRun?
- Tasks are executed via TaskRun
- TaskRun provides inputs required for Task to execute
- in other words, TaskRun is running instance of Task

## What is a Pipeline?
- Tekton Pipeline is a series of Tasks executed in a particular orders
- Tasks can be executed in sequence or in paralled based on your need

## What is a PipelineRun?
- PipelineRun executes a Pipeline providing necessary inputs
- PipelineRun is a running instance of a Pipeline

## What are Workspaces?
- Workspaces are basically Volumes(directory)
- Workspaces are used by a Task to retrieve inputs and store outputs
- Workspaces can be mounted from a ConfigMap, Secret, Persistent Volume Claim or an emptyDir volume

## ⛹️‍♂️ Lab - Listing the operators
```
oc get operators
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>oc get operators</b>
NAME                                                              AGE
ansible-automation-platform-operator.ansible-automation-platfor   3d22h
metallb-operator.metallb-system                                   6d2h
openshift-pipelines-operator-rh.openshift-operators               50m
</pre>

## ⛹️‍♂️ Lab - Listing all the resource of Tekton operators
```
oc get all -n openshift-operators
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>oc get all -n openshift-operators</b>
NAME                                               READY   STATUS    RESTARTS   AGE
pod/openshift-pipelines-operator-77d7d5f85-m5gs8   2/2     Running   0          49m
pod/tekton-operator-webhook-7d6dbcbb6f-m7xbz       1/1     Running   0          49m

NAME                              TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)    AGE
service/tekton-operator           ClusterIP   172.30.91.237   <none>        9090/TCP   49m
service/tekton-operator-webhook   ClusterIP   172.30.180.24   <none>        443/TCP    49m

NAME                                           READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/openshift-pipelines-operator   1/1     1            1           49m
deployment.apps/tekton-operator-webhook        1/1     1            1           49m

NAME                                                     DESIRED   CURRENT   READY   AGE
replicaset.apps/openshift-pipelines-operator-77d7d5f85   1         1         1       49m
replicaset.apps/tekton-operator-webhook-7d6dbcbb6f       1         1         1       49m
</pre>

## ⛹️‍♂️ Lab - Understanding Pod restart policy

### pod.yml
```
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
  labels:
    app: ubuntu
spec:
  restartPolicy: Never
  containers:
  - name: c1
    image: ubuntu
    command:
    - echo
    args:
    - "Hello Pod !"
```

Running the pod
```
oc apply -f pod.yml
oc get po -w
```

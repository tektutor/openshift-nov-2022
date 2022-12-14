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
oc logs my-pod
```

## ⛹️‍♂️ Lab - Creating your first Tekton Task

Create a file named task.yml with below content
```
apiVersion: tekton.dev/v1beta1
kind: Task
metadata:
  name: hello
spec:
  steps:
  - name: echo
    image: ubuntu
    command: 
    - echo
    args:
    - "Hello Tekton !"
```

Running your first Tekton task
```
oc apply -f task.yml
tkn task ls
tkn task start hello
tkn taskrun logs --last
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>oc apply -f task.yml</b>
task.tekton.dev/hello created
(jegan@tektutor.org)$ <b>oc get tasks</b>
NAME    AGE
hello   4s
(jegan@tektutor.org)$ <b>oc get task</b>
NAME    AGE
hello   5s
(jegan@tektutor.org)$ <b>tkn tasks list</b>
NAME    DESCRIPTION   AGE
hello                 13 seconds ago
(jegan@tektutor.org)$ <b>tkn tasks ls</b>
NAME    DESCRIPTION   AGE
hello                 16 seconds ago
(jegan@tektutor.org)$ <b>oc describe task/hello</b>
Name:         hello
Namespace:    jegan
Labels:       <none>
Annotations:  <none>
API Version:  tekton.dev/v1beta1
Kind:         Task
Metadata:
  Creation Timestamp:  2022-11-15T06:06:52Z
  Generation:          1
  Managed Fields:
    API Version:  tekton.dev/v1beta1
    Fields Type:  FieldsV1
    fieldsV1:
      f:metadata:
        f:annotations:
          .:
          f:kubectl.kubernetes.io/last-applied-configuration:
      f:spec:
        .:
        f:steps:
    Manager:         kubectl-client-side-apply
    Operation:       Update
    Time:            2022-11-15T06:06:52Z
  Resource Version:  4638609
  UID:               922a259d-709f-43be-aebf-4409a59cf34a
Spec:
  Steps:
    Args:
      Hello Tekton !
    Command:
      echo
    Image:  ubuntu
    Name:   echo
    Resources:
Events:  <none>
(jegan@tektutor.org)$ <b>tkn task ls</b>
NAME    DESCRIPTION   AGE
hello                 44 seconds ago
(jegan@tektutor.org)$ <b>tkn task start hello</b>
TaskRun started: hello-run-bwpmn

In order to track the TaskRun progress run:
tkn taskrun logs hello-run-bwpmn -f -n jegan
(jegan@tektutor.org)$ <b>tkn taskrun logs hello-run-bwpmn -f -n jegan</b>
[echo] Hello Tekton !
</pre>

## ⛹️‍♂️ Lab - Task that accepts parameters

task.yml
```
apiVersion: tekton.dev/v1beta1
kind: Task
metadata:
  name: hello-task-with-params
spec:
  params:
  - name: message
    type: string
    description: this is a user-defined variable that accepts any message
    default: "Hello Tekton Task !"
  steps:
  - name: step1
    image: ubuntu
    command: 
    - echo
    args:
    - $(params.message)
```

Running the task
```
oc apply -f task.yml
tkn task start hello-task-with-params
tkn tr logs -f --last
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>tkn taskrun logs hello-task-with-params-run-zb59b -f -n jegan</b>
[step1] Hello Tekton Task !

(jegan@tektutor.org)$ <b>tkn task start hello-task-with-params</b>
? Value for param `message` of type `string`? (Default is `Hello Tekton Task !`) Hello World!
TaskRun started: hello-task-with-params-run-82h4f

In order to track the TaskRun progress run:
tkn taskrun logs hello-task-with-params-run-82h4f -f -n jegan
(jegan@tektutor.org)$ <b>tkn taskrun logs -f --last</b>
[step1] Hello World!

(jegan@tektutor.org)$ <b>tkn taskrun ls</b>
NAME                               STARTED          DURATION   STATUS
hello-task-with-params-run-82h4f   28 seconds ago   10s        Succeeded
hello-task-with-params-run-zb59b   57 seconds ago   9s         Succeeded
example-taskrun                    8 minutes ago    28s        Succeeded
hello-run-bwpmn                    48 minutes ago   17s        Succeeded
(jegan@tektutor.org)$ <b>tkn tr logs hello-task-with-params-run-zb59b</b>
[step1] Hello Tekton Task !

(jegan@tektutor.org)$ <b>tkn tr logs hello-task-with-params-run-82h4f</b>
[step1] Hello World!
</pre>

## ⛹️‍♂️ Lab - Deleting Task and TaskRun
```
tkn task delete hello-task-with-multiple-params
tkn tr delete taskrun-multiple-params
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>tkn task delete hello-task-with-multiple-params</b>
Are you sure you want to delete Task(s) "hello-task-with-multiple-params" (y/n): <b>y</b>
Tasks deleted: "hello-task-with-multiple-params"
(jegan@tektutor.org)$ <b>tkn tr ls</b>
NAME                      STARTED         DURATION   STATUS
taskrun-multiple-params   7 minutes ago   9s         Succeeded
(jegan@tektutor.org)$ <b>tkn tr delete taskrun-multiple-params</b>
Are you sure you want to delete TaskRun(s) "taskrun-multiple-params" (y/n): <b>y</b>
TaskRuns deleted: "taskrun-multiple-params"
</pre>

## ⛹️‍♂️ Lab - Task that accepts multiple parameters with a declarative TaskRun
```
cd ~/openshift-nov-2022
git pull
cd Day7/task-with-params

oc apply -f task-with-multiple-params.yml
oc apply -f taskrun.yml

tkn task ls
tkn tr logs -f --last
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>oc apply -f task</b>
taskrun.yml                    task-with-multiple-params.yml  task.yml
(jegan@tektutor.org)$ <b>oc apply -f task-with-multiple-params.yml</b>
task.tekton.dev/hello-task-with-multiple-params created
(jegan@tektutor.org)$ <b>oc apply -f taskrun.yml</b>
taskrun.tekton.dev/taskrun-multiple-params created
(jegan@tektutor.org)$ <b>tkn task ls</b>
NAME                              DESCRIPTION   AGE
hello-task-with-multiple-params                 9 seconds ago
(jegan@tektutor.org)$ <b>tkn tr ls</b>
NAME                      STARTED          DURATION   STATUS
taskrun-multiple-params   10 seconds ago   10s        Succeeded
(jegan@tektutor.org)$ <b>tkn tr logs -f --last</b>
[step1] Hello World !
</pre>

## ⛹️‍♂️ Lab - Task with multiple step
```
cd ~/openshift-nov-2022
git pull
cd Day7/task-with-multiple-steps/

oc apply -f task.yml
tkn t ls
tkn t start task-with-multiple-steps
tkn tr ls
tkn tr logs -f --last
```

The expected output
<pre>
(jegan@tektutor.org)$ <b>oc apply -f task.yml</b>
task.tekton.dev/task-with-multiple-steps created
(jegan@tektutor.org)$ <b>tkn t ls</b>
NAME                              DESCRIPTION   AGE
hello-task-with-multiple-params                 8 minutes ago
task-with-multiple-steps                        4 seconds ago
(jegan@tektutor.org)$ <b>tkn tr ls</b>
NAME                      STARTED         DURATION   STATUS
taskrun-multiple-params   8 minutes ago   10s        Succeeded
(jegan@tektutor.org)$ <b>tkn t start task-with-multiple-steps</b>
TaskRun started: task-with-multiple-steps-run-mcfn4

In order to track the TaskRun progress run:
tkn taskrun logs task-with-multiple-steps-run-mcfn4 -f -n jegan

(jegan@tektutor.org)$ <b>tkn tr logs -f --last</b>
[step-1] Step 1 => Hello TekTon !

[step-2] Step 2 => Hello TekTon !

[step-3] Step 3 => Hello TekTon !
</pre>


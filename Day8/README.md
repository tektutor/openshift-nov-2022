## ⛹️‍♂️ Lab - Creating configmap from file and from-literal
```
oc create configmap my-configmap1 --from-literal=jdk_path=/usr/lib/java --from-literal=m2_path=/usr/share/maven
oc create configmap my-configmap2 --from-file=./app.properties
```

## ⛹️‍♂️ Lab - Creating a TaskRun that maps ConfigMap values in a Workspace directory as files
```
cd ~/openshift-nov-2022
git pull

cd Day8/task-with-configmap
oc apply -f configmap.yml
oc create -f taskrun.yml
tkn tr ls
tkn tr logs -f --last
```

## ⛹️‍♂️ Lab - Creating a TaskRun that maps Secrets values in a Workspace directory as files
```
cd ~/openshift-nov-2022
git pull

cd Day8/task-with-secrets
oc apply -f secrets.yml
oc create -f taskrun.yml
tkn tr ls
tkn tr logs -f --last
```

## ⛹️‍♂️ Lab - TekTon Task that uses Persistent volumes
```
cd ~/openshift-nov-2022
git pull

cd Day8/task-with-persistenvolume
oc apply -f pv.yml
oc apply -f pvc.yml
oc create -f taskrun.yml
tkn tr list
tkn tr logs -f --last
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>ls</b>
pvc.yml  pv.yml  taskrun.yml

(jegan@tektutor.org)$ <b>oc apply -f pv.yml</b>
persistentvolume/tekton-pv-jegan created

(jegan@tektutor.org)$ <b>oc apply -f pvc.yml</b>
persistentvolumeclaim/tekton-pvc-jegan created

(jegan@tektutor.org)$ <b>oc get pvc</b>
NAME               STATUS   VOLUME            CAPACITY   ACCESS MODES   STORAGECLASS   AGE
tekton-pvc-jegan   Bound    tekton-pv-jegan   5Mi        RWX                           5s

(jegan@tektutor.org)$ <b>ls</b>
pvc.yml  pv.yml  taskrun.yml

(jegan@tektutor.org)$ <b>oc create -f taskrun.yml</b>
taskrun.tekton.dev/taskrun-with-pv-jpp4p created

(jegan@tektutor.org)$ <b>tkn tr ls</b>
NAME                           STARTED          DURATION   STATUS
taskrun-with-pv-jpp4p          4 seconds ago    ---        Running(Pending)
taskrun-with-secrets-clvd6     23 minutes ago   10s        Succeeded
taskrun-with-secrets-h8jl7     24 minutes ago   0s         Failed(TaskRunValidationFailed)
taskrun-with-configmap-2phpt   1 hour ago       9s         Succeeded
taskrun-with-configmap-62twx   1 hour ago       10s        Succeeded
taskrun-with-configmap-xbg4w   1 hour ago       9s         Failed
taskrun-with-configmap-8gknj   1 hour ago       15s        Succeeded
taskrun-with-configmap-2nxbc   2 hours ago      20s        Succeeded

(jegan@tektutor.org)$ <b>oc create -f taskrun.yml</b>
taskrun.tekton.dev/taskrun-with-pv-ng7ng created

(jegan@tektutor.org)$ <b>tkn tr logs -f --last</b>
[task-with-pv] Stored the workspace path in a file named path.txt

[print-path] /my/myworkspace
</pre>

## ⛹️‍♂️ Lab - Using emptydir to share data between steps within the same Task
EmptyDir is a temp directory(volume) created on the Task level(i.e Pod).
All the containers running within the same Pod can access the emptyDir volume.

```
cd ~/openshift-nov-2022
git pull
cd Day8/task-with-emptydir

oc create -f taskrun.yml
tkn tr list
tkn tr logs -f --last
```

Expected output
<pre>
jegan@tektutor.org)$ <b>oc create -f taskrun.yml</b>
taskrun.tekton.dev/taskrun-with-emptydir-w8dxt created

(jegan@tektutor.org)$ <b>tkn tr logs -f --last</b>

[step-2] Created by step-1
</pre>

## ⛹️‍♂️ Lab - Creating your first Tekton pipeline
```
cd ~/openshift-nov-2022
git pull

cd Day8/pipelines
oc apply -f first-pipeline.yml
oc get pipelines
tkn pipelines ls
tkn pipeline start first-pipeline --showlog
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>oc apply -f first-pipeline.yml</b>
task.tekton.dev/task1 configured
task.tekton.dev/task2 configured
pipeline.tekton.dev/first-pipeline created

(jegan@tektutor.org)$ <b>oc get pipelines</b>
NAME             AGE
first-pipeline   20s

(jegan@tektutor.org)$ <b>tkn pipelines list</b>
NAME             AGE              LAST RUN                   STARTED         DURATION   STATUS
first-pipeline   27 seconds ago   first-pipeline-run-94qsm   2 minutes ago   32s        Succeeded

(jegan@tektutor.org)$ <b>tkn pipelines ls</b>
NAME             AGE              LAST RUN                   STARTED         DURATION   STATUS
first-pipeline   29 seconds ago   first-pipeline-run-94qsm   2 minutes ago   32s        Succeeded

(jegan@tektutor.org)$ <b>tkn p ls</b>
NAME             AGE              LAST RUN                   STARTED         DURATION   STATUS
first-pipeline   33 seconds ago   first-pipeline-run-94qsm   2 minutes ago   32s        Succeeded

jegan@tektutor.org)$ <b>tkn pipeline start first-pipeline --showlog</b>
PipelineRun started: first-pipeline-run-94qsm
Waiting for logs to be available...
[pipeline-task1 : step-1] Task1 - Step1

[pipeline-task1 : step-2] Task1 - Step2

[pipeline-task2 : step-1] Task2 - Step1

[pipeline-task2 : step-2] Task2 - Step2

[pipeline-task2 : step-3] Task2 - Step3
</pre>

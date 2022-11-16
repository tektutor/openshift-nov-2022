## Creating configmap from file and from-literal
```
oc create configmap my-configmap1 --from-literal=jdk_path=/usr/lib/java --from-literal=m2_path=/usr/share/maven
oc create configmap my-configmap2 --from-file=./app.properties
```

## Creating a TaskRun that maps ConfigMap values in a Workspace directory as files
```
cd ~/openshift-nov-2022
git pull

cd Day8/task-with-configmap
oc apply -f configmap.yml
oc create -f taskrun.yml
tkn tr ls
tkn tr logs -f --last
```

## Creating a TaskRun that maps Secrets values in a Workspace directory as files
```
cd ~/openshift-nov-2022
git pull

cd Day8/task-with-secrets
oc apply -f secrets.yml
oc create -f taskrun.yml
tkn tr ls
tkn tr logs -f --last
```

## TekTon Task that uses Persistent volumes
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
(jegan@tektutor.org)$ ls
pvc.yml  pv.yml  taskrun.yml

(jegan@tektutor.org)$ oc apply -f pv.yml 
persistentvolume/tekton-pv-jegan created

(jegan@tektutor.org)$ oc apply -f pvc.yml 
persistentvolumeclaim/tekton-pvc-jegan created

(jegan@tektutor.org)$ oc get pvc
NAME               STATUS   VOLUME            CAPACITY   ACCESS MODES   STORAGECLASS   AGE
tekton-pvc-jegan   Bound    tekton-pv-jegan   5Mi        RWX                           5s

(jegan@tektutor.org)$ ls
pvc.yml  pv.yml  taskrun.yml

(jegan@tektutor.org)$ oc create -f taskrun.yml 
taskrun.tekton.dev/taskrun-with-pv-jpp4p created

(jegan@tektutor.org)$ tkn tr ls
NAME                           STARTED          DURATION   STATUS
taskrun-with-pv-jpp4p          4 seconds ago    ---        Running(Pending)
taskrun-with-secrets-clvd6     23 minutes ago   10s        Succeeded
taskrun-with-secrets-h8jl7     24 minutes ago   0s         Failed(TaskRunValidationFailed)
taskrun-with-configmap-2phpt   1 hour ago       9s         Succeeded
taskrun-with-configmap-62twx   1 hour ago       10s        Succeeded
taskrun-with-configmap-xbg4w   1 hour ago       9s         Failed
taskrun-with-configmap-8gknj   1 hour ago       15s        Succeeded
taskrun-with-configmap-2nxbc   2 hours ago      20s        Succeeded

(jegan@tektutor.org)$ vim taskrun.yml 
(jegan@tektutor.org)$ oc create -f taskrun.yml 
taskrun.tekton.dev/taskrun-with-pv-ng7ng created

(jegan@tektutor.org)$ tkn tr logs -f --last
[task-with-pv] Stored the workspace path in a file named path.txt

[print-path] /my/myworkspace
</pre>
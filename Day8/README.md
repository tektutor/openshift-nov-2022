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
``

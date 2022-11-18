# Day 10

## You wish to practice OpenShift but don't have an high-end machine then try this
<pre>
https://developers.redhat.com/courses/foundations/getting-started
</pre>
If the cluster is left idle it will destroy the cluster. This setup is good to try one exercise at a time, once each lab exercise is done, you may let it destroy the cluster and recreate the cluster. It will take only few seconds to bring up a new cluster.


## Lab - Tekton CI/CD Pipeline - Build and Deploy

In this lab exercise you will learn how to build a SpringBoot microservice source code, create application binary, build a Container image, pushing the image to Docker Hub and deploying the application using the newly built image.

```
cd ~/openshift-nov-2022-batch2
git pull
cd Day10/cicd
oc project

oc create -f my-scc.yaml
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>oc create -f my-scc.yml</b>
securitycontextconstraints.security.openshift.io/my-scc created
</pre>

#### Create a Custom Security Account
```
cd ~/openshift-nov-2022-batch2
git pull
cd Day10/cicd/
oc project

oc create serviceaccount fsgroup-runasany
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>oc create serviceaccount fsgroup-runasany</b>
serviceaccount/fsgroup-runasany created
</pre>


#### Associate my-scc with the fsgroup-runasany service account
```
oc adm policy add-scc-to-user my-scc -z fsgroup-runasany
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>oc adm policy add-scc-to-user my-scc -z fsgroup-runasany</b>
clusterrole.rbac.authorization.k8s.io/system:openshift:scc:my-scc added: "fsgroup-runasany"
</pre>

#### Associate the privileged SCC with fsgroup-runasany service account
```
oc adm policy add-scc-to-user privileged -z fsgroup-runasany
```
Expected outupt
<pre>
(jegan@tektutor.org)$ <b>oc adm policy add-scc-to-user privileged -z fsgroup-runasany</b>
clusterrole.rbac.authorization.k8s.io/system:openshift:scc:privileged added: "fsgroup-runasany"
</pre>

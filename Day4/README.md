# Day 4

## Create a nginx deployment
```
oc new-app bitnami/nginx:latest
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>oc new-app bitnami/nginx:latest</b>
--> Found container image 82fc406 (34 hours old) from Docker Hub for "bitnami/nginx:latest"

    * An image stream tag will be created as "nginx:latest" that will track this image

--> Creating resources ...
    imagestream.image.openshift.io "nginx" created
    deployment.apps "nginx" created
    service "nginx" created
--> Success
    Application is not exposed. You can expose services to the outside world by executing one or more of the commands below:
     'oc expose service/nginx' 
    Run 'oc status' to view your app.
(jegan@tektutor.org)$ oc status
In project jegan on server https://api.ocp.tektutor.org:6443

svc/nginx - 172.30.66.200 ports 8080, 8443
  deployment/nginx deploys istag/nginx:latest 
    deployment #2 running for 6 seconds - 1 pod
    deployment #1 deployed 9 seconds ago


1 info identified, use 'oc status --suggest' to see details.
</pre>

## Create a NodePort external service
In the below command, the port 8080 represents the ports used by the nginx webserver internally.
A NodePort within the range 30000 to 32767 whichever is available on all the nodes in the cluster will be used by the NodePort Port.  OpenShift automatically verifies the port chosen is not taken already.
```
oc expose deploy/nginx --type=NodePort --port=8080
oc describe svc/nginx
```

Expected output
<pre>
(jegan@tektutor.org)$ oc expose deploy/nginx --type=NodePort --port=8080
service/nginx exposed

(jegan@tektutor.org)$ oc describe svc/nginx
Name:                     nginx
Namespace:                jegan
Labels:                   app=nginx
                          app.kubernetes.io/component=nginx
                          app.kubernetes.io/instance=nginx
Annotations:              <none>
Selector:                 deployment=nginx
Type:                     NodePort
IP Family Policy:         SingleStack
IP Families:              IPv4
IP:                       172.30.119.42
IPs:                      172.30.119.42
Port:                     <unset>  8080/TCP
TargetPort:               8080/TCP
NodePort:                 <unset>  31585/TCP
Endpoints:                10.128.2.35:8080
Session Affinity:         None
External Traffic Policy:  Cluster
Events:                   <none>
</pre>


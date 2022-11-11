# Day 5

## If you haven't cloned this repository, you may do it now
```
cd ~
git clone https://github.com/tektutor/openshift-nov-2022.git
```

## Using a multi stage dockerfile to build and deploy your java application

You need to build this image before you can push the image to your Docker Hub ( all the below steps can be skipped as I have already pushed the image to my Docker Hub )
```
cd ~/openshift-nov-2022
git pull
cd Day5/multi-stage-dockerimage
docker build -t tektutor/spring-tektuotr-helloms:latest .
docker login
docker push tektutor/spring-tektuotr-helloms:latest
```

Deploying your custom java microservice from Docker Hub image we pushed just now
```
oc new-app tektutor/spring-tektutor-helloms:latest
```

<pre>
(jegan@tektutor.org)$ <b>oc new-app tektutor/spring-tektutor-helloms:latest</b>
--> Found container image 43265ee (9 minutes old) from Docker Hub for "tektutor/spring-tektutor-helloms:latest"

    Java Applications 
    ----------------- 
    Platform for building and running plain Java applications (fat-jar and flat classpath)

    Tags: builder, java

    * An image stream tag will be created as "spring-tektutor-helloms:latest" that will track this image

--> Creating resources ...
    imagestream.image.openshift.io "spring-tektutor-helloms" created
    deployment.apps "spring-tektutor-helloms" created
    service "spring-tektutor-helloms" created
--> Success
    Application is not exposed. You can expose services to the outside world by executing one or more of the commands below:
     'oc expose service/spring-tektutor-helloms' 
    Run 'oc status' to view your app.
(jegan@tektutor.org)$ oc status
In project TekTutor OpenShift project (jegan) on server https://api.ocp.tektutor.org:6443

svc/spring-tektutor-helloms - 172.30.11.2 ports 8080, 8443, 8778
  deployment/spring-tektutor-helloms deploys istag/spring-tektutor-helloms:latest 
    deployment #1 running for 3 seconds - 0/1 pods growing to 1


1 info identified, use 'oc status --suggest' to see details.
(jegan@tektutor.org)$ <b>oc get bc</b>
No resources found in jegan namespace.
(jegan@tektutor.org)$ <b>oc get svc</b>
NAME                      TYPE        CLUSTER-IP    EXTERNAL-IP   PORT(S)                      AGE
spring-tektutor-helloms   ClusterIP   172.30.11.2   <none>        8080/TCP,8443/TCP,8778/TCP   53s
(jegan@tektutor.org)$ <b>oc expose svc/spring-tektutor-helloms</b>
route.route.openshift.io/spring-tektutor-helloms exposed
(jegan@tektutor.org)$ <b>oc get route</b>
NAME                      HOST/PORT                                             PATH   SERVICES                  PORT       TERMINATION   WILDCARD
spring-tektutor-helloms   spring-tektutor-helloms-jegan.apps.ocp.tektutor.org          spring-tektutor-helloms   8080-tcp                 None
(jegan@tektutor.org)$ <b>curl spring-tektutor-helloms-jegan.apps.ocp.tektutor.org</b>
<b>Hello Java Microsrvice !</b>
</pre>

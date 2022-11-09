# Day 3

## Lab - Getting inside a Pod using its deployment name
```
oc rsh deploy/nginx
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>oc rsh deploy/nginx</b>
$ <b>hostname</b>
nginx-78644964b4-wbn48
$ <b>hostname -i</b>
10.131.0.26
$ <b>exit</b>
</pre>

## Lab -  Getting inside a specific Pod shell
```
oc get po
oc rsh nginx-78644964b4-jg7wz
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>oc get po</b>
NAME                     READY   STATUS    RESTARTS   AGE
nginx-78644964b4-jg7wz   1/1     Running   0          4m38s
nginx-78644964b4-wbn48   1/1     Running   0          5m36s
nginx-78644964b4-zvzc6   1/1     Running   0          5m36s
(jegan@tektutor.org)$ <b>oc rsh nginx-78644964b4-jg7wz</b>
$ <b>hostname</b>
nginx-78644964b4-jg7wz
$ <b>hostname -i</b>
10.130.1.81
$ <b>exit</b>
</pre>

## Deploying an application from GitHub source code
```
oc delete deploy/nginx

oc new-app --name=hello registry.redhat.io/ubi8/openjdk-11:latest~https://github.com/tektutor/hello-springboot-ms.git

oc logs -f bc/hello

oc expose svc/hello
```

Expected output
<pre>
(jegan@tektutor.org)$ oc new-app --name=hello registry.redhat.io/ubi8/openjdk-11:latest~https://github.com/tektutor/hello-spring-boot.git
--> Found container image 0fb9f36 (2 weeks old) from registry.redhat.io for "registry.redhat.io/ubi8/openjdk-11:latest"

    Java Applications 
    ----------------- 
    Platform for building and running plain Java applications (fat-jar and flat classpath)

    Tags: builder, java

    * An image stream tag will be created as "openjdk-11:latest" that will track the source image
    * A source build using source code from https://github.com/tektutor/hello-spring-boot.git will be created
      * The resulting image will be pushed to image stream tag "hello:latest"
      * Every time "openjdk-11:latest" changes a new build will be triggered

--> Creating resources ...
    imagestream.image.openshift.io "hello" created
    buildconfig.build.openshift.io "hello" created
    deployment.apps "hello" created
    service "hello" created
--> Success
    Build scheduled, use 'oc logs -f buildconfig/hello' to track its progress.
    Application is not exposed. You can expose services to the outside world by executing one or more of the commands below:
     'oc expose service/hello' 
    Run 'oc status' to view your app.
</pre>

Creating an external route to access your application
```
oc expose svc/hello
oc get route
```

Expected output
<pre>
(jegan@tektutor.org)$ oc get route
NAME    HOST/PORT                           PATH   SERVICES   PORT       TERMINATION   WILDCARD
hello   hello-jegan.apps.ocp.tektutor.org          hello      8080-tcp                 None
(jegan@tektutor.org)$ curl hello-jegan.apps.ocp.tektutor.org
Greetings from Spring Boot!

</pre>

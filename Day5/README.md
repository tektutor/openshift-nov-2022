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
Expected output ( Many lines are removed to keep it short )
<pre>
(jegan@tektutor.org)$ docker build -t tektutor/spring-hello-ms:1.0 .
Sending build context to Docker daemon  8.704kB
Step 1/9 : FROM registry.redhat.io/ubi8/openjdk-11:latest as builder
 ---> 0c308464b19e
Step 2/9 : MAINTAINER Jeganathan Swaminathan <jegan@tektutor.org>
 ---> Using cache
 ---> 18d740613f9b
Step 3/9 : RUN mkdir -p -m 0700 ./hello/target
 ---> Using cache
 ---> 2653212e0640
Step 4/9 : WORKDIR ./hello
 ---> Using cache
 ---> 566bf25b5f09
Step 5/9 : COPY . ./
 ---> 4bd9e6edafa1
Step 6/9 : RUN mvn package && cp ./target/tektutor-hello-microservice-1.0.jar /tmp/app.jar
 ---> Running in 8f35466aadc9
[INFO] Scanning for projects...
Downloading from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-starter-parent/2.4.2/spring-boot-starter-parent-2.4.2.pom
Downloaded from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-starter-parent/2.4.2/spring-boot-starter-parent-2.4.2.pom (8.6 kB at 6.0 kB/s)
Downloading from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-dependencies/2.4.2/spring-boot-dependencies-2.4.2.pom
Downloaded from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-dependencies/2.4.2/spring-boot-dependencies-2.4.2.pom (108 kB at 123 kB/s)
Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-classworlds/2.6.0/plexus-classworlds-2.6.0.jar
Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-utils/3.2.1/plexus-utils-3.2.1.jar (262 kB at 130 kB/s)
Downloaded from central: https://repo1.maven.org/maven2/org/eclipse/sisu/org.eclipse.sisu.inject/0.3.4/org.eclipse.sisu.inject-0.3.4.jar (379 kB at 188 kB/s)
Downloaded from central: https://repo1.maven.org/maven2/org/eclipse/sisu/org.eclipse.sisu.plexus/0.3.4/org.eclipse.sisu.plexus-0.3.4.jar (205 kB at 101 kB/s)
Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-classworlds/2.6.0/plexus-classworlds-2.6.0.jar (53 kB at 23 kB/s)
[INFO] Replacing main artifact with repackaged archive
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  02:58 min
[INFO] Finished at: 2022-11-11T06:07:17Z
[INFO] ------------------------------------------------------------------------
Removing intermediate container 8f35466aadc9
 ---> 651afeea6f03
Step 7/9 : FROM registry.redhat.io/ubi8/openjdk-11:latest as runner
 ---> 0c308464b19e
Step 8/9 : COPY --from=builder /tmp/app.jar .
 ---> 138effb2f9ca
Step 9/9 : CMD ["java", "-jar", "./app.jar"]
 ---> Running in c061ed3f1d9e
Removing intermediate container c061ed3f1d9e
 ---> b95efbf727c6
Successfully built b95efbf727c6
Successfully tagged tektutor/spring-hello-ms:1.0

(jegan@tektutor.org)$ <b>docker login</b>
Authenticating with existing credentials...
WARNING! Your password will be stored unencrypted in /home/jegan/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded
(jegan@tektutor.org)$ <b>docker tag tektutor/spring-hello-ms:1.0 tektutor/spring-tektutor-helloms:latest</b>
(jegan@tektutor.org)$ <b>docker push tektutor/spring-tektutor-helloms:latest</b>
The push refers to repository [docker.io/tektutor/spring-tektutor-helloms]
5f7cf289fea9: Pushed 
69c55192bed9: Layer already exists 
8e0e04b5c700: Layer already exists 
647a854c512b: Layer already exists 
latest: digest: sha256:d1652ad0441e0da8c5ef6947f47a7cf1eeddb55d67770e5d04d143cfee3a63fb size: 1162
</pre>

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

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
(jegan@tektutor.org)$ <b>oc new-app --name=hello registry.redhat.io/ubi8/openjdk-11:latest~https://github.com/tektutor/hello-spring-boot.git</b>
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

Checking the build config logs
```
oc logs -f bc/hello
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>oc logs -f bc/hello</b>
Cloning "https://github.com/tektutor/hello-spring-boot.git" ...
	Commit:	638c7cbce33e8ea894a8c1ba853a2c09502368eb (Set up CI with Azure Pipelines)
	Author:	Jeganathan Swaminathan <jegan@tektutor.org>
	Date:	Fri Oct 21 19:48:59 2022 +0530
time="2022-11-09T04:43:49Z" level=info msg="Not using native diff for overlay, this may cause degraded performance for building images: kernel has CONFIG_OVERLAY_FS_REDIRECT_DIR enabled"
I1109 04:43:49.767573       1 defaults.go:102] Defaulting to storage driver "overlay" with options [mountopt=metacopy=on].
Caching blobs under "/var/cache/blobs".
Trying to pull registry.redhat.io/ubi8/openjdk-11@sha256:44fabdc44dc062c9aa80b352e515defcb7976b7bc339947d9fe79c4aefdd89d9...
Getting image source signatures
Copying blob sha256:550a30ba276ce28ef23d8f3b23ec19faa4173b8c9a197f2331c5df0f8fd9f7b9
Copying blob sha256:488d0614fc378af3bd3dc61a94e61987e86d74a480b8fe0048d8a7e7574202de
Copying config sha256:0fb9f36a8eead5061ed4bd9d895245b4c58eef89ef8c942de829857a9c776671
Writing manifest to image destination
Storing signatures
Generating dockerfile with builder image registry.redhat.io/ubi8/openjdk-11@sha256:44fabdc44dc062c9aa80b352e515defcb7976b7bc339947d9fe79c4aefdd89d9
Adding transient rw bind mount for /run/secrets/rhsm
STEP 1/9: FROM registry.redhat.io/ubi8/openjdk-11@sha256:44fabdc44dc062c9aa80b352e515defcb7976b7bc339947d9fe79c4aefdd89d9
STEP 2/9: LABEL "io.openshift.build.commit.ref"="main"       "io.openshift.build.commit.message"="Set up CI with Azure Pipelines"       "io.openshift.build.source-location"="https://github.com/tektutor/hello-spring-boot.git"       "io.openshift.s2i.destination"="/tmp"       "io.openshift.build.image"="registry.redhat.io/ubi8/openjdk-11@sha256:44fabdc44dc062c9aa80b352e515defcb7976b7bc339947d9fe79c4aefdd89d9"       "io.openshift.build.commit.author"="Jeganathan Swaminathan <jegan@tektutor.org>"       "io.openshift.build.commit.date"="Fri Oct 21 19:48:59 2022 +0530"       "io.openshift.build.commit.id"="638c7cbce33e8ea894a8c1ba853a2c09502368eb"
STEP 3/9: ENV OPENSHIFT_BUILD_NAME="hello-1"     OPENSHIFT_BUILD_NAMESPACE="jegan"     OPENSHIFT_BUILD_SOURCE="https://github.com/tektutor/hello-spring-boot.git"     OPENSHIFT_BUILD_COMMIT="638c7cbce33e8ea894a8c1ba853a2c09502368eb"
STEP 4/9: USER root
STEP 5/9: COPY upload/src /tmp/src
STEP 6/9: RUN chown -R 185:0 /tmp/src
STEP 7/9: USER 185
STEP 8/9: RUN /usr/local/s2i/assemble
INFO Performing Maven build in /tmp/src
INFO Using MAVEN_OPTS -XX:+UseParallelGC -XX:MinHeapFreeRatio=10 -XX:MaxHeapFreeRatio=20 -XX:GCTimeRatio=4 -XX:AdaptiveSizePolicyWeight=90 -XX:+ExitOnOutOfMemoryError
INFO Using Apache Maven 3.6.2 (Red Hat 3.6.2-7)
Maven home: /usr/share/maven
Java version: 11.0.17, vendor: Red Hat, Inc., runtime: /usr/lib/jvm/java-11-openjdk-11.0.17.0.8-2.el8_6.x86_64
Default locale: en, platform encoding: UTF-8
OS name: "linux", version: "4.18.0-305.62.1.el8_4.x86_64", arch: "amd64", family: "unix"
INFO Running 'mvn -e -Popenshift -DskipTests -Dcom.redhat.xpaas.repo.redhatga -Dfabric8.skip=true -Djkube.skip=true --batch-mode -Djava.net.preferIPv4Stack=true -s /tmp/artifacts/configuration/settings.xml -Dmaven.repo.local=/tmp/artifacts/m2  package'
[INFO] Error stacktraces are turned on.
[INFO] Scanning for projects...
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-starter-parent/2.4.2/spring-boot-starter-parent-2.4.2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-starter-parent/2.4.2/spring-boot-starter-parent-2.4.2.pom (8.6 kB at 7.2 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-dependencies/2.4.2/spring-boot-dependencies-2.4.2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-dependencies/2.4.2/spring-boot-dependencies-2.4.2.pom (108 kB at 170 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/com/datastax/oss/java-driver-bom/4.9.0/java-driver-bom-4.9.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/com/datastax/oss/java-driver-bom/4.9.0/java-driver-bom-4.9.0.pom (4.1 kB at 12 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/io/dropwizard/metrics/metrics-bom/4.1.17/metrics-bom-4.1.17.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/io/dropwizard/metrics/metrics-bom/4.1.17/metrics-bom-4.1.17.pom (5.3 kB at 15 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/io/dropwizard/metrics/metrics-parent/4.1.17/metrics-parent-4.1.17.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/io/dropwizard/metrics/metrics-parent/4.1.17/metrics-parent-4.1.17.pom (17 kB at 46 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/groovy/groovy-bom/2.5.14/groovy-bom-2.5.14.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/groovy/groovy-bom/2.5.14/groovy-bom-2.5.14.pom (26 kB at 70 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/infinispan/infinispan-bom/11.0.8.Final/infinispan-bom-11.0.8.Final.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/infinispan/infinispan-bom/11.0.8.Final/infinispan-bom-11.0.8.Final.pom (19 kB at 51 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/infinispan/infinispan-build-configuration-parent/11.0.8.Final/infinispan-build-configuration-parent-11.0.8.Final.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/infinispan/infinispan-build-configuration-parent/11.0.8.Final/infinispan-build-configuration-parent-11.0.8.Final.pom (13 kB at 36 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/jboss/jboss-parent/36/jboss-parent-36.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/jboss/jboss-parent/36/jboss-parent-36.pom (66 kB at 165 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/com/fasterxml/jackson/jackson-bom/2.11.4/jackson-bom-2.11.4.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/com/fasterxml/jackson/jackson-bom/2.11.4/jackson-bom-2.11.4.pom (15 kB at 39 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/com/fasterxml/jackson/jackson-parent/2.11/jackson-parent-2.11.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/com/fasterxml/jackson/jackson-parent/2.11/jackson-parent-2.11.pom (7.8 kB at 22 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/com/fasterxml/oss-parent/38/oss-parent-38.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/com/fasterxml/oss-parent/38/oss-parent-38.pom (23 kB at 62 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/glassfish/jersey/jersey-bom/2.32/jersey-bom-2.32.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/glassfish/jersey/jersey-bom/2.32/jersey-bom-2.32.pom (19 kB at 52 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/eclipse/ee4j/project/1.0.6/project-1.0.6.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/eclipse/ee4j/project/1.0.6/project-1.0.6.pom (13 kB at 36 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/eclipse/jetty/jetty-bom/9.4.35.v20201120/jetty-bom-9.4.35.v20201120.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/eclipse/jetty/jetty-bom/9.4.35.v20201120/jetty-bom-9.4.35.v20201120.pom (18 kB at 48 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/junit/junit-bom/5.7.0/junit-bom-5.7.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/junit/junit-bom/5.7.0/junit-bom-5.7.0.pom (5.1 kB at 14 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/jetbrains/kotlin/kotlin-bom/1.4.21/kotlin-bom-1.4.21.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/jetbrains/kotlin/kotlin-bom/1.4.21/kotlin-bom-1.4.21.pom (9.3 kB at 26 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/jetbrains/kotlinx/kotlinx-coroutines-bom/1.4.2/kotlinx-coroutines-bom-1.4.2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/jetbrains/kotlinx/kotlinx-coroutines-bom/1.4.2/kotlinx-coroutines-bom-1.4.2.pom (4.1 kB at 12 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/logging/log4j/log4j-bom/2.13.3/log4j-bom-2.13.3.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/logging/log4j/log4j-bom/2.13.3/log4j-bom-2.13.3.pom (7.6 kB at 21 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/logging/logging-parent/1/logging-parent-1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/logging/logging-parent/1/logging-parent-1.pom (3.2 kB at 8.9 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/apache/18/apache-18.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/apache/18/apache-18.pom (16 kB at 43 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/io/micrometer/micrometer-bom/1.6.3/micrometer-bom-1.6.3.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/io/micrometer/micrometer-bom/1.6.3/micrometer-bom-1.6.3.pom (6.8 kB at 19 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/io/netty/netty-bom/4.1.58.Final/netty-bom-4.1.58.Final.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/io/netty/netty-bom/4.1.58.Final/netty-bom-4.1.58.Final.pom (8.8 kB at 24 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/sonatype/oss/oss-parent/7/oss-parent-7.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/sonatype/oss/oss-parent/7/oss-parent-7.pom (4.8 kB at 13 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/com/oracle/database/jdbc/ojdbc-bom/19.8.0.0/ojdbc-bom-19.8.0.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/com/oracle/database/jdbc/ojdbc-bom/19.8.0.0/ojdbc-bom-19.8.0.0.pom (12 kB at 34 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/io/r2dbc/r2dbc-bom/Arabba-SR8/r2dbc-bom-Arabba-SR8.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/io/r2dbc/r2dbc-bom/Arabba-SR8/r2dbc-bom-Arabba-SR8.pom (4.1 kB at 10 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/io/projectreactor/reactor-bom/2020.0.3/reactor-bom-2020.0.3.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/io/projectreactor/reactor-bom/2020.0.3/reactor-bom-2020.0.3.pom (4.5 kB at 12 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/io/rsocket/rsocket-bom/1.1.0/rsocket-bom-1.1.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/io/rsocket/rsocket-bom/1.1.0/rsocket-bom-1.1.0.pom (2.6 kB at 7.4 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/data/spring-data-bom/2020.0.3/spring-data-bom-2020.0.3.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/data/spring-data-bom/2020.0.3/spring-data-bom-2020.0.3.pom (5.9 kB at 16 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/spring-framework-bom/5.3.3/spring-framework-bom-5.3.3.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/spring-framework-bom/5.3.3/spring-framework-bom-5.3.3.pom (5.6 kB at 16 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/integration/spring-integration-bom/5.4.3/spring-integration-bom-5.4.3.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/integration/spring-integration-bom/5.4.3/spring-integration-bom-5.4.3.pom (9.5 kB at 26 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/security/spring-security-bom/5.4.2/spring-security-bom-5.4.2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/security/spring-security-bom/5.4.2/spring-security-bom-5.4.2.pom (5.3 kB at 14 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/session/spring-session-bom/2020.0.1/spring-session-bom-2020.0.1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/session/spring-session-bom/2020.0.1/spring-session-bom-2020.0.1.pom (2.7 kB at 7.6 kB/s)
[INFO] 
[INFO] ---------------------< org.tektutor:spring-hello >----------------------
[INFO] Building spring-boot 1.3
[INFO] --------------------------------[ jar ]---------------------------------
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-maven-plugin/2.4.2/spring-boot-maven-plugin-2.4.2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-maven-plugin/2.4.2/spring-boot-maven-plugin-2.4.2.pom (2.9 kB at 8.2 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-maven-plugin/2.4.2/spring-boot-maven-plugin-2.4.2.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-maven-plugin/2.4.2/spring-boot-maven-plugin-2.4.2.jar (101 kB at 239 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/plugins/maven-resources-plugin/3.2.0/maven-resources-plugin-3.2.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/plugins/maven-resources-plugin/3.2.0/maven-resources-plugin-3.2.0.pom (8.1 kB at 22 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/plugins/maven-plugins/34/maven-plugins-34.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/plugins/maven-plugins/34/maven-plugins-34.pom (11 kB at 30 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-parent/34/maven-parent-34.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-parent/34/maven-parent-34.pom (43 kB at 115 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/apache/23/apache-23.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/apache/23/apache-23.pom (18 kB at 51 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/plugins/maven-resources-plugin/3.2.0/maven-resources-plugin-3.2.0.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/plugins/maven-resources-plugin/3.2.0/maven-resources-plugin-3.2.0.jar (33 kB at 89 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/plugins/maven-compiler-plugin/3.8.1/maven-compiler-plugin-3.8.1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/plugins/maven-compiler-plugin/3.8.1/maven-compiler-plugin-3.8.1.pom (12 kB at 34 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/plugins/maven-plugins/33/maven-plugins-33.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/plugins/maven-plugins/33/maven-plugins-33.pom (11 kB at 30 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-parent/33/maven-parent-33.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-parent/33/maven-parent-33.pom (44 kB at 118 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/apache/21/apache-21.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/apache/21/apache-21.pom (17 kB at 47 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/plugins/maven-compiler-plugin/3.8.1/maven-compiler-plugin-3.8.1.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/plugins/maven-compiler-plugin/3.8.1/maven-compiler-plugin-3.8.1.jar (62 kB at 163 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/plugins/maven-surefire-plugin/2.22.2/maven-surefire-plugin-2.22.2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/plugins/maven-surefire-plugin/2.22.2/maven-surefire-plugin-2.22.2.pom (5.0 kB at 14 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/surefire/surefire/2.22.2/surefire-2.22.2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/surefire/surefire/2.22.2/surefire-2.22.2.pom (26 kB at 70 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/plugins/maven-surefire-plugin/2.22.2/maven-surefire-plugin-2.22.2.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/plugins/maven-surefire-plugin/2.22.2/maven-surefire-plugin-2.22.2.jar (41 kB at 109 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/plugins/maven-jar-plugin/3.2.0/maven-jar-plugin-3.2.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/plugins/maven-jar-plugin/3.2.0/maven-jar-plugin-3.2.0.pom (7.3 kB at 20 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/plugins/maven-jar-plugin/3.2.0/maven-jar-plugin-3.2.0.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/plugins/maven-jar-plugin/3.2.0/maven-jar-plugin-3.2.0.jar (29 kB at 77 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-starter-web/2.4.2/spring-boot-starter-web-2.4.2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-starter-web/2.4.2/spring-boot-starter-web-2.4.2.pom (3.0 kB at 8.3 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-starter/2.4.2/spring-boot-starter-2.4.2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-starter/2.4.2/spring-boot-starter-2.4.2.pom (3.1 kB at 7.8 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot/2.4.2/spring-boot-2.4.2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot/2.4.2/spring-boot-2.4.2.pom (2.2 kB at 5.4 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/spring-core/5.3.3/spring-core-5.3.3.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/spring-core/5.3.3/spring-core-5.3.3.pom (2.0 kB at 3.8 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/spring-jcl/5.3.3/spring-jcl-5.3.3.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/spring-jcl/5.3.3/spring-jcl-5.3.3.pom (1.8 kB at 5.2 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/spring-context/5.3.3/spring-context-5.3.3.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/spring-context/5.3.3/spring-context-5.3.3.pom (2.6 kB at 6.5 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/spring-aop/5.3.3/spring-aop-5.3.3.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/spring-aop/5.3.3/spring-aop-5.3.3.pom (2.2 kB at 6.2 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/spring-beans/5.3.3/spring-beans-5.3.3.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/spring-beans/5.3.3/spring-beans-5.3.3.pom (2.0 kB at 5.3 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/spring-expression/5.3.3/spring-expression-5.3.3.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/spring-expression/5.3.3/spring-expression-5.3.3.pom (2.1 kB at 5.2 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-autoconfigure/2.4.2/spring-boot-autoconfigure-2.4.2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-autoconfigure/2.4.2/spring-boot-autoconfigure-2.4.2.pom (2.1 kB at 5.8 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-starter-logging/2.4.2/spring-boot-starter-logging-2.4.2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-starter-logging/2.4.2/spring-boot-starter-logging-2.4.2.pom (2.5 kB at 6.9 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/ch/qos/logback/logback-classic/1.2.3/logback-classic-1.2.3.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/ch/qos/logback/logback-classic/1.2.3/logback-classic-1.2.3.pom (13 kB at 36 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/ch/qos/logback/logback-parent/1.2.3/logback-parent-1.2.3.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/ch/qos/logback/logback-parent/1.2.3/logback-parent-1.2.3.pom (18 kB at 49 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/ch/qos/logback/logback-core/1.2.3/logback-core-1.2.3.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/ch/qos/logback/logback-core/1.2.3/logback-core-1.2.3.pom (4.2 kB at 12 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/slf4j/slf4j-api/1.7.30/slf4j-api-1.7.30.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/slf4j/slf4j-api/1.7.30/slf4j-api-1.7.30.pom (3.8 kB at 11 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/slf4j/slf4j-parent/1.7.30/slf4j-parent-1.7.30.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/slf4j/slf4j-parent/1.7.30/slf4j-parent-1.7.30.pom (14 kB at 38 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/logging/log4j/log4j-to-slf4j/2.13.3/log4j-to-slf4j-2.13.3.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/logging/log4j/log4j-to-slf4j/2.13.3/log4j-to-slf4j-2.13.3.pom (7.2 kB at 20 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/logging/log4j/log4j/2.13.3/log4j-2.13.3.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/logging/log4j/log4j/2.13.3/log4j-2.13.3.pom (64 kB at 166 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/logging/log4j/log4j-api/2.13.3/log4j-api-2.13.3.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/logging/log4j/log4j-api/2.13.3/log4j-api-2.13.3.pom (13 kB at 36 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/slf4j/jul-to-slf4j/1.7.30/jul-to-slf4j-1.7.30.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/slf4j/jul-to-slf4j/1.7.30/jul-to-slf4j-1.7.30.pom (990 B at 2.8 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/jakarta/annotation/jakarta.annotation-api/1.3.5/jakarta.annotation-api-1.3.5.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/jakarta/annotation/jakarta.annotation-api/1.3.5/jakarta.annotation-api-1.3.5.pom (16 kB at 43 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/jakarta/annotation/ca-parent/1.3.5/ca-parent-1.3.5.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/jakarta/annotation/ca-parent/1.3.5/ca-parent-1.3.5.pom (2.8 kB at 7.9 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/eclipse/ee4j/project/1.0.5/project-1.0.5.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/eclipse/ee4j/project/1.0.5/project-1.0.5.pom (13 kB at 37 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/yaml/snakeyaml/1.27/snakeyaml-1.27.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/yaml/snakeyaml/1.27/snakeyaml-1.27.pom (37 kB at 100 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-starter-json/2.4.2/spring-boot-starter-json-2.4.2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-starter-json/2.4.2/spring-boot-starter-json-2.4.2.pom (3.1 kB at 8.6 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/spring-web/5.3.3/spring-web-5.3.3.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/spring-web/5.3.3/spring-web-5.3.3.pom (2.2 kB at 6.2 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/com/fasterxml/jackson/core/jackson-databind/2.11.4/jackson-databind-2.11.4.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/com/fasterxml/jackson/core/jackson-databind/2.11.4/jackson-databind-2.11.4.pom (7.4 kB at 21 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/com/fasterxml/jackson/jackson-base/2.11.4/jackson-base-2.11.4.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/com/fasterxml/jackson/jackson-base/2.11.4/jackson-base-2.11.4.pom (7.5 kB at 21 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/com/fasterxml/jackson/core/jackson-annotations/2.11.4/jackson-annotations-2.11.4.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/com/fasterxml/jackson/core/jackson-annotations/2.11.4/jackson-annotations-2.11.4.pom (4.6 kB at 13 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/com/fasterxml/jackson/core/jackson-core/2.11.4/jackson-core-2.11.4.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/com/fasterxml/jackson/core/jackson-core/2.11.4/jackson-core-2.11.4.pom (4.9 kB at 14 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/com/fasterxml/jackson/datatype/jackson-datatype-jdk8/2.11.4/jackson-datatype-jdk8-2.11.4.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/com/fasterxml/jackson/datatype/jackson-datatype-jdk8/2.11.4/jackson-datatype-jdk8-2.11.4.pom (2.2 kB at 6.2 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/com/fasterxml/jackson/module/jackson-modules-java8/2.11.4/jackson-modules-java8-2.11.4.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/com/fasterxml/jackson/module/jackson-modules-java8/2.11.4/jackson-modules-java8-2.11.4.pom (3.2 kB at 8.7 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/com/fasterxml/jackson/datatype/jackson-datatype-jsr310/2.11.4/jackson-datatype-jsr310-2.11.4.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/com/fasterxml/jackson/datatype/jackson-datatype-jsr310/2.11.4/jackson-datatype-jsr310-2.11.4.pom (4.5 kB at 13 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/com/fasterxml/jackson/module/jackson-module-parameter-names/2.11.4/jackson-module-parameter-names-2.11.4.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/com/fasterxml/jackson/module/jackson-module-parameter-names/2.11.4/jackson-module-parameter-names-2.11.4.pom (4.0 kB at 11 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-starter-tomcat/2.4.2/spring-boot-starter-tomcat-2.4.2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-starter-tomcat/2.4.2/spring-boot-starter-tomcat-2.4.2.pom (3.1 kB at 8.7 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/tomcat/embed/tomcat-embed-core/9.0.41/tomcat-embed-core-9.0.41.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/tomcat/embed/tomcat-embed-core/9.0.41/tomcat-embed-core-9.0.41.pom (1.8 kB at 5.0 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/glassfish/jakarta.el/3.0.3/jakarta.el-3.0.3.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/glassfish/jakarta.el/3.0.3/jakarta.el-3.0.3.pom (14 kB at 30 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/tomcat/embed/tomcat-embed-websocket/9.0.41/tomcat-embed-websocket-9.0.41.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/tomcat/embed/tomcat-embed-websocket/9.0.41/tomcat-embed-websocket-9.0.41.pom (1.8 kB at 5.0 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/spring-webmvc/5.3.3/spring-webmvc-5.3.3.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/spring-webmvc/5.3.3/spring-webmvc-5.3.3.pom (2.9 kB at 8.3 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-starter-test/2.4.2/spring-boot-starter-test-2.4.2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-starter-test/2.4.2/spring-boot-starter-test-2.4.2.pom (4.7 kB at 13 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-test/2.4.2/spring-boot-test-2.4.2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-test/2.4.2/spring-boot-test-2.4.2.pom (2.1 kB at 5.7 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-test-autoconfigure/2.4.2/spring-boot-test-autoconfigure-2.4.2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-test-autoconfigure/2.4.2/spring-boot-test-autoconfigure-2.4.2.pom (2.5 kB at 7.0 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/com/jayway/jsonpath/json-path/2.4.0/json-path-2.4.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/com/jayway/jsonpath/json-path/2.4.0/json-path-2.4.0.pom (2.6 kB at 7.4 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/net/minidev/json-smart/2.3/json-smart-2.3.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/net/minidev/json-smart/2.3/json-smart-2.3.pom (9.0 kB at 25 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/net/minidev/minidev-parent/2.3/minidev-parent-2.3.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/net/minidev/minidev-parent/2.3/minidev-parent-2.3.pom (8.5 kB at 24 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/net/minidev/accessors-smart/1.2/accessors-smart-1.2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/net/minidev/accessors-smart/1.2/accessors-smart-1.2.pom (12 kB at 32 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/ow2/asm/asm/5.0.4/asm-5.0.4.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/ow2/asm/asm/5.0.4/asm-5.0.4.pom (1.9 kB at 5.4 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/ow2/asm/asm-parent/5.0.4/asm-parent-5.0.4.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/ow2/asm/asm-parent/5.0.4/asm-parent-5.0.4.pom (5.5 kB at 15 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/ow2/ow2/1.3/ow2-1.3.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/ow2/ow2/1.3/ow2-1.3.pom (9.5 kB at 26 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/jakarta/xml/bind/jakarta.xml.bind-api/2.3.3/jakarta.xml.bind-api-2.3.3.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/jakarta/xml/bind/jakarta.xml.bind-api/2.3.3/jakarta.xml.bind-api-2.3.3.pom (13 kB at 37 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/jakarta/xml/bind/jakarta.xml.bind-api-parent/2.3.3/jakarta.xml.bind-api-parent-2.3.3.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/jakarta/xml/bind/jakarta.xml.bind-api-parent/2.3.3/jakarta.xml.bind-api-parent-2.3.3.pom (9.0 kB at 25 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/jakarta/activation/jakarta.activation-api/1.2.2/jakarta.activation-api-1.2.2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/jakarta/activation/jakarta.activation-api/1.2.2/jakarta.activation-api-1.2.2.pom (5.3 kB at 15 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/com/sun/activation/all/1.2.2/all-1.2.2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/com/sun/activation/all/1.2.2/all-1.2.2.pom (15 kB at 42 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/assertj/assertj-core/3.18.1/assertj-core-3.18.1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/assertj/assertj-core/3.18.1/assertj-core-3.18.1.pom (24 kB at 66 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/assertj/assertj-parent-pom/2.2.8/assertj-parent-pom-2.2.8.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/assertj/assertj-parent-pom/2.2.8/assertj-parent-pom-2.2.8.pom (24 kB at 66 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/junit/junit-bom/5.6.3/junit-bom-5.6.3.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/junit/junit-bom/5.6.3/junit-bom-5.6.3.pom (4.9 kB at 14 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/hamcrest/hamcrest/2.2/hamcrest-2.2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/hamcrest/hamcrest/2.2/hamcrest-2.2.pom (1.1 kB at 3.2 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/junit/jupiter/junit-jupiter/5.7.0/junit-jupiter-5.7.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/junit/jupiter/junit-jupiter/5.7.0/junit-jupiter-5.7.0.pom (3.2 kB at 9.0 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/junit/jupiter/junit-jupiter-api/5.7.0/junit-jupiter-api-5.7.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/junit/jupiter/junit-jupiter-api/5.7.0/junit-jupiter-api-5.7.0.pom (3.2 kB at 8.9 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apiguardian/apiguardian-api/1.1.0/apiguardian-api-1.1.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apiguardian/apiguardian-api/1.1.0/apiguardian-api-1.1.0.pom (1.2 kB at 3.3 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/opentest4j/opentest4j/1.2.0/opentest4j-1.2.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/opentest4j/opentest4j/1.2.0/opentest4j-1.2.0.pom (1.7 kB at 4.8 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/junit/platform/junit-platform-commons/1.7.0/junit-platform-commons-1.7.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/junit/platform/junit-platform-commons/1.7.0/junit-platform-commons-1.7.0.pom (2.8 kB at 7.8 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/junit/jupiter/junit-jupiter-params/5.7.0/junit-jupiter-params-5.7.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/junit/jupiter/junit-jupiter-params/5.7.0/junit-jupiter-params-5.7.0.pom (3.0 kB at 8.3 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/junit/jupiter/junit-jupiter-engine/5.7.0/junit-jupiter-engine-5.7.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/junit/jupiter/junit-jupiter-engine/5.7.0/junit-jupiter-engine-5.7.0.pom (3.2 kB at 9.0 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/junit/platform/junit-platform-engine/1.7.0/junit-platform-engine-1.7.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/junit/platform/junit-platform-engine/1.7.0/junit-platform-engine-1.7.0.pom (3.2 kB at 9.0 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/mockito/mockito-core/3.6.28/mockito-core-3.6.28.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/mockito/mockito-core/3.6.28/mockito-core-3.6.28.pom (2.9 kB at 8.1 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/net/bytebuddy/byte-buddy/1.10.19/byte-buddy-1.10.19.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/net/bytebuddy/byte-buddy/1.10.19/byte-buddy-1.10.19.pom (11 kB at 30 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/net/bytebuddy/byte-buddy-parent/1.10.19/byte-buddy-parent-1.10.19.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/net/bytebuddy/byte-buddy-parent/1.10.19/byte-buddy-parent-1.10.19.pom (42 kB at 98 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/net/bytebuddy/byte-buddy-agent/1.10.19/byte-buddy-agent-1.10.19.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/net/bytebuddy/byte-buddy-agent/1.10.19/byte-buddy-agent-1.10.19.pom (9.6 kB at 26 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/objenesis/objenesis/3.1/objenesis-3.1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/objenesis/objenesis/3.1/objenesis-3.1.pom (3.5 kB at 10.0 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/objenesis/objenesis-parent/3.1/objenesis-parent-3.1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/objenesis/objenesis-parent/3.1/objenesis-parent-3.1.pom (18 kB at 49 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/mockito/mockito-junit-jupiter/3.6.28/mockito-junit-jupiter-3.6.28.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/mockito/mockito-junit-jupiter/3.6.28/mockito-junit-jupiter-3.6.28.pom (2.7 kB at 7.6 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/skyscreamer/jsonassert/1.5.0/jsonassert-1.5.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/skyscreamer/jsonassert/1.5.0/jsonassert-1.5.0.pom (5.2 kB at 15 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/com/vaadin/external/google/android-json/0.0.20131108.vaadin1/android-json-0.0.20131108.vaadin1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/com/vaadin/external/google/android-json/0.0.20131108.vaadin1/android-json-0.0.20131108.vaadin1.pom (2.8 kB at 7.8 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/spring-test/5.3.3/spring-test-5.3.3.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/spring-test/5.3.3/spring-test-5.3.3.pom (2.0 kB at 5.8 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/xmlunit/xmlunit-core/2.7.0/xmlunit-core-2.7.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/xmlunit/xmlunit-core/2.7.0/xmlunit-core-2.7.0.pom (2.7 kB at 7.6 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/xmlunit/xmlunit-parent/2.7.0/xmlunit-parent-2.7.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/xmlunit/xmlunit-parent/2.7.0/xmlunit-parent-2.7.0.pom (20 kB at 53 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-starter-web/2.4.2/spring-boot-starter-web-2.4.2.jar
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-autoconfigure/2.4.2/spring-boot-autoconfigure-2.4.2.jar
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-starter-logging/2.4.2/spring-boot-starter-logging-2.4.2.jar
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot/2.4.2/spring-boot-2.4.2.jar
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-starter/2.4.2/spring-boot-starter-2.4.2.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-starter-logging/2.4.2/spring-boot-starter-logging-2.4.2.jar (4.8 kB at 6.4 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/ch/qos/logback/logback-classic/1.2.3/logback-classic-1.2.3.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-starter-web/2.4.2/spring-boot-starter-web-2.4.2.jar (4.8 kB at 6.1 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/ch/qos/logback/logback-core/1.2.3/logback-core-1.2.3.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-starter/2.4.2/spring-boot-starter-2.4.2.jar (4.8 kB at 5.9 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/logging/log4j/log4j-to-slf4j/2.13.3/log4j-to-slf4j-2.13.3.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-autoconfigure/2.4.2/spring-boot-autoconfigure-2.4.2.jar (1.5 MB at 1.7 MB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/logging/log4j/log4j-api/2.13.3/log4j-api-2.13.3.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/logging/log4j/log4j-to-slf4j/2.13.3/log4j-to-slf4j-2.13.3.jar (17 kB at 14 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/slf4j/jul-to-slf4j/1.7.30/jul-to-slf4j-1.7.30.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/logging/log4j/log4j-api/2.13.3/log4j-api-2.13.3.jar (292 kB at 226 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/jakarta/annotation/jakarta.annotation-api/1.3.5/jakarta.annotation-api-1.3.5.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/ch/qos/logback/logback-classic/1.2.3/logback-classic-1.2.3.jar (290 kB at 184 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/yaml/snakeyaml/1.27/snakeyaml-1.27.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/slf4j/jul-to-slf4j/1.7.30/jul-to-slf4j-1.7.30.jar (4.6 kB at 2.9 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-starter-json/2.4.2/spring-boot-starter-json-2.4.2.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/jakarta/annotation/jakarta.annotation-api/1.3.5/jakarta.annotation-api-1.3.5.jar (25 kB at 15 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/com/fasterxml/jackson/core/jackson-databind/2.11.4/jackson-databind-2.11.4.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot/2.4.2/spring-boot-2.4.2.jar (1.3 MB at 771 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/com/fasterxml/jackson/core/jackson-annotations/2.11.4/jackson-annotations-2.11.4.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/ch/qos/logback/logback-core/1.2.3/logback-core-1.2.3.jar (472 kB at 270 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/com/fasterxml/jackson/core/jackson-core/2.11.4/jackson-core-2.11.4.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-starter-json/2.4.2/spring-boot-starter-json-2.4.2.jar (4.7 kB at 2.3 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/com/fasterxml/jackson/datatype/jackson-datatype-jdk8/2.11.4/jackson-datatype-jdk8-2.11.4.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/yaml/snakeyaml/1.27/snakeyaml-1.27.jar (310 kB at 153 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/com/fasterxml/jackson/datatype/jackson-datatype-jsr310/2.11.4/jackson-datatype-jsr310-2.11.4.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/com/fasterxml/jackson/core/jackson-annotations/2.11.4/jackson-annotations-2.11.4.jar (72 kB at 34 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/com/fasterxml/jackson/module/jackson-module-parameter-names/2.11.4/jackson-module-parameter-names-2.11.4.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/com/fasterxml/jackson/core/jackson-databind/2.11.4/jackson-databind-2.11.4.jar (1.4 MB at 655 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-starter-tomcat/2.4.2/spring-boot-starter-tomcat-2.4.2.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/com/fasterxml/jackson/core/jackson-core/2.11.4/jackson-core-2.11.4.jar (352 kB at 159 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/tomcat/embed/tomcat-embed-core/9.0.41/tomcat-embed-core-9.0.41.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/com/fasterxml/jackson/datatype/jackson-datatype-jsr310/2.11.4/jackson-datatype-jsr310-2.11.4.jar (111 kB at 46 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/glassfish/jakarta.el/3.0.3/jakarta.el-3.0.3.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/com/fasterxml/jackson/datatype/jackson-datatype-jdk8/2.11.4/jackson-datatype-jdk8-2.11.4.jar (34 kB at 14 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/tomcat/embed/tomcat-embed-websocket/9.0.41/tomcat-embed-websocket-9.0.41.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/com/fasterxml/jackson/module/jackson-module-parameter-names/2.11.4/jackson-module-parameter-names-2.11.4.jar (9.3 kB at 3.6 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/spring-web/5.3.3/spring-web-5.3.3.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-starter-tomcat/2.4.2/spring-boot-starter-tomcat-2.4.2.jar (4.8 kB at 1.9 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/spring-beans/5.3.3/spring-beans-5.3.3.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/glassfish/jakarta.el/3.0.3/jakarta.el-3.0.3.jar (238 kB at 81 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/spring-webmvc/5.3.3/spring-webmvc-5.3.3.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/spring-beans/5.3.3/spring-beans-5.3.3.jar (696 kB at 228 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/spring-aop/5.3.3/spring-aop-5.3.3.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/spring-web/5.3.3/spring-web-5.3.3.jar (1.6 MB at 486 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/spring-context/5.3.3/spring-context-5.3.3.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/tomcat/embed/tomcat-embed-websocket/9.0.41/tomcat-embed-websocket-9.0.41.jar (272 kB at 78 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/spring-expression/5.3.3/spring-expression-5.3.3.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/spring-aop/5.3.3/spring-aop-5.3.3.jar (374 kB at 107 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-starter-test/2.4.2/spring-boot-starter-test-2.4.2.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/spring-webmvc/5.3.3/spring-webmvc-5.3.3.jar (996 kB at 278 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-test/2.4.2/spring-boot-test-2.4.2.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/spring-context/5.3.3/spring-context-5.3.3.jar (1.2 MB at 331 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-test-autoconfigure/2.4.2/spring-boot-test-autoconfigure-2.4.2.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-starter-test/2.4.2/spring-boot-starter-test-2.4.2.jar (4.8 kB at 1.2 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/com/jayway/jsonpath/json-path/2.4.0/json-path-2.4.0.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/tomcat/embed/tomcat-embed-core/9.0.41/tomcat-embed-core-9.0.41.jar (3.4 MB at 883 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/net/minidev/json-smart/2.3/json-smart-2.3.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-test/2.4.2/spring-boot-test-2.4.2.jar (219 kB at 55 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/net/minidev/accessors-smart/1.2/accessors-smart-1.2.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-test-autoconfigure/2.4.2/spring-boot-test-autoconfigure-2.4.2.jar (182 kB at 44 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/ow2/asm/asm/5.0.4/asm-5.0.4.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/spring-expression/5.3.3/spring-expression-5.3.3.jar (283 kB at 68 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/slf4j/slf4j-api/1.7.30/slf4j-api-1.7.30.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/net/minidev/json-smart/2.3/json-smart-2.3.jar (120 kB at 28 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/jakarta/xml/bind/jakarta.xml.bind-api/2.3.3/jakarta.xml.bind-api-2.3.3.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/com/jayway/jsonpath/json-path/2.4.0/json-path-2.4.0.jar (223 kB at 52 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/jakarta/activation/jakarta.activation-api/1.2.2/jakarta.activation-api-1.2.2.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/net/minidev/accessors-smart/1.2/accessors-smart-1.2.jar (30 kB at 6.9 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/assertj/assertj-core/3.18.1/assertj-core-3.18.1.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/slf4j/slf4j-api/1.7.30/slf4j-api-1.7.30.jar (41 kB at 9.2 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/hamcrest/hamcrest/2.2/hamcrest-2.2.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/ow2/asm/asm/5.0.4/asm-5.0.4.jar (53 kB at 12 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/junit/jupiter/junit-jupiter/5.7.0/junit-jupiter-5.7.0.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/jakarta/xml/bind/jakarta.xml.bind-api/2.3.3/jakarta.xml.bind-api-2.3.3.jar (116 kB at 25 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/junit/jupiter/junit-jupiter-api/5.7.0/junit-jupiter-api-5.7.0.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/jakarta/activation/jakarta.activation-api/1.2.2/jakarta.activation-api-1.2.2.jar (47 kB at 10 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apiguardian/apiguardian-api/1.1.0/apiguardian-api-1.1.0.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/hamcrest/hamcrest/2.2/hamcrest-2.2.jar (123 kB at 25 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/opentest4j/opentest4j/1.2.0/opentest4j-1.2.0.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/junit/jupiter/junit-jupiter/5.7.0/junit-jupiter-5.7.0.jar (6.4 kB at 1.3 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/junit/platform/junit-platform-commons/1.7.0/junit-platform-commons-1.7.0.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/junit/jupiter/junit-jupiter-api/5.7.0/junit-jupiter-api-5.7.0.jar (175 kB at 34 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/junit/jupiter/junit-jupiter-params/5.7.0/junit-jupiter-params-5.7.0.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apiguardian/apiguardian-api/1.1.0/apiguardian-api-1.1.0.jar (2.4 kB at 461 B/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/junit/jupiter/junit-jupiter-engine/5.7.0/junit-jupiter-engine-5.7.0.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/assertj/assertj-core/3.18.1/assertj-core-3.18.1.jar (4.8 MB at 924 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/junit/platform/junit-platform-engine/1.7.0/junit-platform-engine-1.7.0.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/opentest4j/opentest4j/1.2.0/opentest4j-1.2.0.jar (7.7 kB at 1.4 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/mockito/mockito-core/3.6.28/mockito-core-3.6.28.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/junit/platform/junit-platform-commons/1.7.0/junit-platform-commons-1.7.0.jar (100 kB at 19 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/net/bytebuddy/byte-buddy/1.10.19/byte-buddy-1.10.19.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/junit/jupiter/junit-jupiter-params/5.7.0/junit-jupiter-params-5.7.0.jar (567 kB at 103 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/net/bytebuddy/byte-buddy-agent/1.10.19/byte-buddy-agent-1.10.19.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/junit/jupiter/junit-jupiter-engine/5.7.0/junit-jupiter-engine-5.7.0.jar (212 kB at 38 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/objenesis/objenesis/3.1/objenesis-3.1.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/junit/platform/junit-platform-engine/1.7.0/junit-platform-engine-1.7.0.jar (181 kB at 32 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/mockito/mockito-junit-jupiter/3.6.28/mockito-junit-jupiter-3.6.28.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/mockito/mockito-core/3.6.28/mockito-core-3.6.28.jar (675 kB at 117 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/skyscreamer/jsonassert/1.5.0/jsonassert-1.5.0.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/net/bytebuddy/byte-buddy-agent/1.10.19/byte-buddy-agent-1.10.19.jar (259 kB at 44 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/com/vaadin/external/google/android-json/0.0.20131108.vaadin1/android-json-0.0.20131108.vaadin1.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/objenesis/objenesis/3.1/objenesis-3.1.jar (60 kB at 10 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/spring-core/5.3.3/spring-core-5.3.3.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/mockito/mockito-junit-jupiter/3.6.28/mockito-junit-jupiter-3.6.28.jar (4.9 kB at 809 B/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/spring-jcl/5.3.3/spring-jcl-5.3.3.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/skyscreamer/jsonassert/1.5.0/jsonassert-1.5.0.jar (30 kB at 4.9 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/spring-test/5.3.3/spring-test-5.3.3.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/com/vaadin/external/google/android-json/0.0.20131108.vaadin1/android-json-0.0.20131108.vaadin1.jar (18 kB at 2.9 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/xmlunit/xmlunit-core/2.7.0/xmlunit-core-2.7.0.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/spring-core/5.3.3/spring-core-5.3.3.jar (1.5 MB at 227 kB/s)
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/spring-jcl/5.3.3/spring-jcl-5.3.3.jar (24 kB at 3.7 kB/s)
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/spring-test/5.3.3/spring-test-5.3.3.jar (763 kB at 115 kB/s)
[INFO] Downloaded from central: https://repo1.maven.org/maven2/net/bytebuddy/byte-buddy/1.10.19/byte-buddy-1.10.19.jar (3.5 MB at 525 kB/s)
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/xmlunit/xmlunit-core/2.7.0/xmlunit-core-2.7.0.jar (168 kB at 25 kB/s)
[INFO] 
[INFO] --- maven-resources-plugin:3.2.0:resources (default-resources) @ spring-hello ---
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-plugin-api/3.1.0/maven-plugin-api-3.1.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-plugin-api/3.1.0/maven-plugin-api-3.1.0.pom (3.0 kB at 8.4 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven/3.1.0/maven-3.1.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven/3.1.0/maven-3.1.0.pom (22 kB at 62 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-parent/23/maven-parent-23.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-parent/23/maven-parent-23.pom (33 kB at 92 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/apache/13/apache-13.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/apache/13/apache-13.pom (14 kB at 39 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-model/3.1.0/maven-model-3.1.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-model/3.1.0/maven-model-3.1.0.pom (3.8 kB at 11 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-utils/3.0.10/plexus-utils-3.0.10.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-utils/3.0.10/plexus-utils-3.0.10.pom (3.1 kB at 8.9 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus/3.3/plexus-3.3.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus/3.3/plexus-3.3.pom (20 kB at 57 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/sonatype/spice/spice-parent/17/spice-parent-17.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/sonatype/spice/spice-parent/17/spice-parent-17.pom (6.8 kB at 19 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/sonatype/forge/forge-parent/10/forge-parent-10.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/sonatype/forge/forge-parent/10/forge-parent-10.pom (14 kB at 38 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-artifact/3.1.0/maven-artifact-3.1.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-artifact/3.1.0/maven-artifact-3.1.0.pom (1.6 kB at 4.7 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/eclipse/sisu/org.eclipse.sisu.plexus/0.0.0.M2a/org.eclipse.sisu.plexus-0.0.0.M2a.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/eclipse/sisu/org.eclipse.sisu.plexus/0.0.0.M2a/org.eclipse.sisu.plexus-0.0.0.M2a.pom (5.9 kB at 17 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/eclipse/sisu/sisu-plexus/0.0.0.M2a/sisu-plexus-0.0.0.M2a.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/eclipse/sisu/sisu-plexus/0.0.0.M2a/sisu-plexus-0.0.0.M2a.pom (7.9 kB at 23 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/javax/enterprise/cdi-api/1.0/cdi-api-1.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/javax/enterprise/cdi-api/1.0/cdi-api-1.0.pom (1.4 kB at 4.1 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/jboss/weld/weld-api-parent/1.0/weld-api-parent-1.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/jboss/weld/weld-api-parent/1.0/weld-api-parent-1.0.pom (2.4 kB at 6.7 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/jboss/weld/weld-api-bom/1.0/weld-api-bom-1.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/jboss/weld/weld-api-bom/1.0/weld-api-bom-1.0.pom (7.9 kB at 22 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/jboss/weld/weld-parent/6/weld-parent-6.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/jboss/weld/weld-parent/6/weld-parent-6.pom (21 kB at 59 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/javax/annotation/jsr250-api/1.0/jsr250-api-1.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/javax/annotation/jsr250-api/1.0/jsr250-api-1.0.pom (1.0 kB at 2.9 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/javax/inject/javax.inject/1/javax.inject-1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/javax/inject/javax.inject/1/javax.inject-1.pom (612 B at 1.7 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/com/google/guava/guava/10.0.1/guava-10.0.1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/com/google/guava/guava/10.0.1/guava-10.0.1.pom (5.4 kB at 15 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/com/google/guava/guava-parent/10.0.1/guava-parent-10.0.1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/com/google/guava/guava-parent/10.0.1/guava-parent-10.0.1.pom (2.0 kB at 5.6 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/com/google/code/findbugs/jsr305/1.3.9/jsr305-1.3.9.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/com/google/code/findbugs/jsr305/1.3.9/jsr305-1.3.9.pom (965 B at 2.7 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/sonatype/sisu/sisu-guice/3.1.0/sisu-guice-3.1.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/sonatype/sisu/sisu-guice/3.1.0/sisu-guice-3.1.0.pom (10 kB at 29 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/sonatype/sisu/inject/guice-parent/3.1.0/guice-parent-3.1.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/sonatype/sisu/inject/guice-parent/3.1.0/guice-parent-3.1.0.pom (11 kB at 31 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/aopalliance/aopalliance/1.0/aopalliance-1.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/aopalliance/aopalliance/1.0/aopalliance-1.0.pom (363 B at 1.0 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/eclipse/sisu/org.eclipse.sisu.inject/0.0.0.M2a/org.eclipse.sisu.inject-0.0.0.M2a.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/eclipse/sisu/org.eclipse.sisu.inject/0.0.0.M2a/org.eclipse.sisu.inject-0.0.0.M2a.pom (5.0 kB at 14 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/eclipse/sisu/sisu-inject/0.0.0.M2a/sisu-inject-0.0.0.M2a.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/eclipse/sisu/sisu-inject/0.0.0.M2a/sisu-inject-0.0.0.M2a.pom (7.8 kB at 22 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/asm/asm/3.3.1/asm-3.3.1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/asm/asm/3.3.1/asm-3.3.1.pom (266 B at 760 B/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/asm/asm-parent/3.3.1/asm-parent-3.3.1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/asm/asm-parent/3.3.1/asm-parent-3.3.1.pom (4.3 kB at 12 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-component-annotations/2.0.0/plexus-component-annotations-2.0.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-component-annotations/2.0.0/plexus-component-annotations-2.0.0.pom (750 B at 2.0 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-containers/2.0.0/plexus-containers-2.0.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-containers/2.0.0/plexus-containers-2.0.0.pom (4.8 kB at 14 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus/5.1/plexus-5.1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus/5.1/plexus-5.1.pom (23 kB at 64 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-classworlds/2.4/plexus-classworlds-2.4.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-classworlds/2.4/plexus-classworlds-2.4.pom (3.9 kB at 11 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus/2.0.7/plexus-2.0.7.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus/2.0.7/plexus-2.0.7.pom (17 kB at 49 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-utils/2.1/plexus-utils-2.1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-utils/2.1/plexus-utils-2.1.pom (4.0 kB at 11 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/sonatype/spice/spice-parent/16/spice-parent-16.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/sonatype/spice/spice-parent/16/spice-parent-16.pom (8.4 kB at 22 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/sonatype/forge/forge-parent/5/forge-parent-5.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/sonatype/forge/forge-parent/5/forge-parent-5.pom (8.4 kB at 21 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-core/3.1.0/maven-core-3.1.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-core/3.1.0/maven-core-3.1.0.pom (6.9 kB at 20 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-settings/3.1.0/maven-settings-3.1.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-settings/3.1.0/maven-settings-3.1.0.pom (1.8 kB at 5.2 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-settings-builder/3.1.0/maven-settings-builder-3.1.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-settings-builder/3.1.0/maven-settings-builder-3.1.0.pom (2.3 kB at 6.4 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-interpolation/1.16/plexus-interpolation-1.16.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-interpolation/1.16/plexus-interpolation-1.16.pom (1.0 kB at 2.9 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-components/1.3/plexus-components-1.3.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-components/1.3/plexus-components-1.3.pom (3.1 kB at 8.7 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/sonatype/plexus/plexus-sec-dispatcher/1.3/plexus-sec-dispatcher-1.3.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/sonatype/plexus/plexus-sec-dispatcher/1.3/plexus-sec-dispatcher-1.3.pom (3.0 kB at 8.4 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/sonatype/spice/spice-parent/12/spice-parent-12.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/sonatype/spice/spice-parent/12/spice-parent-12.pom (6.8 kB at 19 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/sonatype/forge/forge-parent/4/forge-parent-4.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/sonatype/forge/forge-parent/4/forge-parent-4.pom (8.4 kB at 24 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-utils/1.5.5/plexus-utils-1.5.5.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-utils/1.5.5/plexus-utils-1.5.5.pom (5.1 kB at 15 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus/1.0.11/plexus-1.0.11.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus/1.0.11/plexus-1.0.11.pom (9.0 kB at 25 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/sonatype/plexus/plexus-cipher/1.4/plexus-cipher-1.4.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/sonatype/plexus/plexus-cipher/1.4/plexus-cipher-1.4.pom (2.1 kB at 5.9 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-repository-metadata/3.1.0/maven-repository-metadata-3.1.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-repository-metadata/3.1.0/maven-repository-metadata-3.1.0.pom (1.9 kB at 5.2 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-model-builder/3.1.0/maven-model-builder-3.1.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-model-builder/3.1.0/maven-model-builder-3.1.0.pom (2.5 kB at 7.0 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-aether-provider/3.1.0/maven-aether-provider-3.1.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-aether-provider/3.1.0/maven-aether-provider-3.1.0.pom (3.5 kB at 9.8 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/eclipse/aether/aether-api/0.9.0.M2/aether-api-0.9.0.M2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/eclipse/aether/aether-api/0.9.0.M2/aether-api-0.9.0.M2.pom (1.7 kB at 4.9 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/eclipse/aether/aether/0.9.0.M2/aether-0.9.0.M2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/eclipse/aether/aether/0.9.0.M2/aether-0.9.0.M2.pom (28 kB at 79 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/eclipse/aether/aether-spi/0.9.0.M2/aether-spi-0.9.0.M2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/eclipse/aether/aether-spi/0.9.0.M2/aether-spi-0.9.0.M2.pom (1.8 kB at 5.0 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/eclipse/aether/aether-util/0.9.0.M2/aether-util-0.9.0.M2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/eclipse/aether/aether-util/0.9.0.M2/aether-util-0.9.0.M2.pom (2.0 kB at 5.8 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/eclipse/aether/aether-impl/0.9.0.M2/aether-impl-0.9.0.M2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/eclipse/aether/aether-impl/0.9.0.M2/aether-impl-0.9.0.M2.pom (3.3 kB at 9.5 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-classworlds/2.4.2/plexus-classworlds-2.4.2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-classworlds/2.4.2/plexus-classworlds-2.4.2.pom (3.5 kB at 10.0 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus/3.0.1/plexus-3.0.1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus/3.0.1/plexus-3.0.1.pom (19 kB at 53 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-interpolation/1.26/plexus-interpolation-1.26.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-interpolation/1.26/plexus-interpolation-1.26.pom (2.7 kB at 7.5 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-filtering/3.2.0/maven-filtering-3.2.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-filtering/3.2.0/maven-filtering-3.2.0.pom (6.8 kB at 19 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-shared-components/34/maven-shared-components-34.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-shared-components/34/maven-shared-components-34.pom (5.1 kB at 15 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-shared-utils/3.3.3/maven-shared-utils-3.3.3.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-shared-utils/3.3.3/maven-shared-utils-3.3.3.pom (5.8 kB at 16 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/commons-io/commons-io/2.6/commons-io-2.6.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/commons-io/commons-io/2.6/commons-io-2.6.pom (14 kB at 40 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/commons/commons-parent/42/commons-parent-42.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/commons/commons-parent/42/commons-parent-42.pom (68 kB at 191 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-utils/3.3.0/plexus-utils-3.3.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-utils/3.3.0/plexus-utils-3.3.0.pom (5.2 kB at 13 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/sonatype/plexus/plexus-build-api/0.0.7/plexus-build-api-0.0.7.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/sonatype/plexus/plexus-build-api/0.0.7/plexus-build-api-0.0.7.pom (3.2 kB at 8.9 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/sonatype/spice/spice-parent/15/spice-parent-15.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/sonatype/spice/spice-parent/15/spice-parent-15.pom (8.4 kB at 24 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-utils/1.5.8/plexus-utils-1.5.8.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-utils/1.5.8/plexus-utils-1.5.8.pom (8.1 kB at 19 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus/2.0.2/plexus-2.0.2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus/2.0.2/plexus-2.0.2.pom (12 kB at 32 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/commons/commons-lang3/3.8.1/commons-lang3-3.8.1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/commons/commons-lang3/3.8.1/commons-lang3-3.8.1.pom (28 kB at 77 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/commons/commons-parent/47/commons-parent-47.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/commons/commons-parent/47/commons-parent-47.pom (78 kB at 213 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/apache/19/apache-19.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/apache/19/apache-19.pom (15 kB at 44 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-plugin-api/3.1.0/maven-plugin-api-3.1.0.jar
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-settings/3.1.0/maven-settings-3.1.0.jar
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-settings-builder/3.1.0/maven-settings-builder-3.1.0.jar
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-artifact/3.1.0/maven-artifact-3.1.0.jar
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-core/3.1.0/maven-core-3.1.0.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-settings-builder/3.1.0/maven-settings-builder-3.1.0.jar (41 kB at 110 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-repository-metadata/3.1.0/maven-repository-metadata-3.1.0.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-settings/3.1.0/maven-settings-3.1.0.jar (47 kB at 124 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-model-builder/3.1.0/maven-model-builder-3.1.0.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-plugin-api/3.1.0/maven-plugin-api-3.1.0.jar (50 kB at 131 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-aether-provider/3.1.0/maven-aether-provider-3.1.0.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-core/3.1.0/maven-core-3.1.0.jar (563 kB at 1.3 MB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/eclipse/aether/aether-spi/0.9.0.M2/aether-spi-0.9.0.M2.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-artifact/3.1.0/maven-artifact-3.1.0.jar (52 kB at 122 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/eclipse/aether/aether-impl/0.9.0.M2/aether-impl-0.9.0.M2.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-repository-metadata/3.1.0/maven-repository-metadata-3.1.0.jar (30 kB at 40 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/eclipse/aether/aether-api/0.9.0.M2/aether-api-0.9.0.M2.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-aether-provider/3.1.0/maven-aether-provider-3.1.0.jar (60 kB at 80 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/eclipse/aether/aether-util/0.9.0.M2/aether-util-0.9.0.M2.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-model-builder/3.1.0/maven-model-builder-3.1.0.jar (159 kB at 211 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-utils/3.0.10/plexus-utils-3.0.10.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/eclipse/aether/aether-spi/0.9.0.M2/aether-spi-0.9.0.M2.jar (18 kB at 23 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-classworlds/2.4.2/plexus-classworlds-2.4.2.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/eclipse/aether/aether-impl/0.9.0.M2/aether-impl-0.9.0.M2.jar (145 kB at 158 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/sonatype/plexus/plexus-sec-dispatcher/1.3/plexus-sec-dispatcher-1.3.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-utils/3.0.10/plexus-utils-3.0.10.jar (231 kB at 202 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/sonatype/plexus/plexus-cipher/1.4/plexus-cipher-1.4.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/eclipse/aether/aether-util/0.9.0.M2/aether-util-0.9.0.M2.jar (134 kB at 116 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-model/3.1.0/maven-model-3.1.0.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/eclipse/aether/aether-api/0.9.0.M2/aether-api-0.9.0.M2.jar (134 kB at 116 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-component-annotations/2.0.0/plexus-component-annotations-2.0.0.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-classworlds/2.4.2/plexus-classworlds-2.4.2.jar (47 kB at 41 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-interpolation/1.26/plexus-interpolation-1.26.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/sonatype/plexus/plexus-sec-dispatcher/1.3/plexus-sec-dispatcher-1.3.jar (29 kB at 22 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/eclipse/sisu/org.eclipse.sisu.plexus/0.0.0.M2a/org.eclipse.sisu.plexus-0.0.0.M2a.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/sonatype/plexus/plexus-cipher/1.4/plexus-cipher-1.4.jar (13 kB at 9.0 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/javax/enterprise/cdi-api/1.0/cdi-api-1.0.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-model/3.1.0/maven-model-3.1.0.jar (164 kB at 108 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/javax/annotation/jsr250-api/1.0/jsr250-api-1.0.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-component-annotations/2.0.0/plexus-component-annotations-2.0.0.jar (4.2 kB at 2.8 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/javax/inject/javax.inject/1/javax.inject-1.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-interpolation/1.26/plexus-interpolation-1.26.jar (85 kB at 56 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/com/google/guava/guava/10.0.1/guava-10.0.1.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/javax/enterprise/cdi-api/1.0/cdi-api-1.0.jar (45 kB at 24 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/com/google/code/findbugs/jsr305/1.3.9/jsr305-1.3.9.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/eclipse/sisu/org.eclipse.sisu.plexus/0.0.0.M2a/org.eclipse.sisu.plexus-0.0.0.M2a.jar (202 kB at 109 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/sonatype/sisu/sisu-guice/3.1.0/sisu-guice-3.1.0-no_aop.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/javax/annotation/jsr250-api/1.0/jsr250-api-1.0.jar (5.8 kB at 3.1 kB/s)
[INFO] Downloaded from central: https://repo1.maven.org/maven2/javax/inject/javax.inject/1/javax.inject-1.jar (2.5 kB at 1.3 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/eclipse/sisu/org.eclipse.sisu.inject/0.0.0.M2a/org.eclipse.sisu.inject-0.0.0.M2a.jar
[INFO] Downloading from central: https://repo1.maven.org/maven2/aopalliance/aopalliance/1.0/aopalliance-1.0.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/com/google/guava/guava/10.0.1/guava-10.0.1.jar (1.5 MB at 753 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/asm/asm/3.3.1/asm-3.3.1.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/com/google/code/findbugs/jsr305/1.3.9/jsr305-1.3.9.jar (33 kB at 15 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-filtering/3.2.0/maven-filtering-3.2.0.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/sonatype/sisu/sisu-guice/3.1.0/sisu-guice-3.1.0-no_aop.jar (357 kB at 160 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-shared-utils/3.3.3/maven-shared-utils-3.3.3.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/aopalliance/aopalliance/1.0/aopalliance-1.0.jar (4.5 kB at 2.0 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/sonatype/plexus/plexus-build-api/0.0.7/plexus-build-api-0.0.7.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/eclipse/sisu/org.eclipse.sisu.inject/0.0.0.M2a/org.eclipse.sisu.inject-0.0.0.M2a.jar (202 kB at 88 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/commons-io/commons-io/2.6/commons-io-2.6.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/asm/asm/3.3.1/asm-3.3.1.jar (44 kB at 19 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/commons/commons-lang3/3.8.1/commons-lang3-3.8.1.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-filtering/3.2.0/maven-filtering-3.2.0.jar (52 kB at 20 kB/s)
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-shared-utils/3.3.3/maven-shared-utils-3.3.3.jar (154 kB at 59 kB/s)
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/sonatype/plexus/plexus-build-api/0.0.7/plexus-build-api-0.0.7.jar (8.5 kB at 3.2 kB/s)
[INFO] Downloaded from central: https://repo1.maven.org/maven2/commons-io/commons-io/2.6/commons-io-2.6.jar (215 kB at 80 kB/s)
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/commons/commons-lang3/3.8.1/commons-lang3-3.8.1.jar (502 kB at 182 kB/s)
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] Using 'UTF-8' encoding to copy filtered properties files.
[INFO] skip non existing resourceDirectory /tmp/src/src/main/resources
[INFO] skip non existing resourceDirectory /tmp/src/src/main/resources
[INFO] 
[INFO] --- maven-compiler-plugin:3.8.1:compile (default-compile) @ spring-hello ---
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-plugin-api/3.0/maven-plugin-api-3.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-plugin-api/3.0/maven-plugin-api-3.0.pom (2.3 kB at 6.4 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven/3.0/maven-3.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven/3.0/maven-3.0.pom (22 kB at 62 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-parent/15/maven-parent-15.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-parent/15/maven-parent-15.pom (24 kB at 68 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/apache/6/apache-6.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/apache/6/apache-6.pom (13 kB at 36 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-model/3.0/maven-model-3.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-model/3.0/maven-model-3.0.pom (3.9 kB at 11 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-utils/2.0.4/plexus-utils-2.0.4.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-utils/2.0.4/plexus-utils-2.0.4.pom (3.3 kB at 9.2 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus/2.0.6/plexus-2.0.6.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus/2.0.6/plexus-2.0.6.pom (17 kB at 47 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-artifact/3.0/maven-artifact-3.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-artifact/3.0/maven-artifact-3.0.pom (1.9 kB at 5.4 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/sonatype/sisu/sisu-inject-plexus/1.4.2/sisu-inject-plexus-1.4.2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/sonatype/sisu/sisu-inject-plexus/1.4.2/sisu-inject-plexus-1.4.2.pom (5.4 kB at 15 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/sonatype/sisu/inject/guice-plexus/1.4.2/guice-plexus-1.4.2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/sonatype/sisu/inject/guice-plexus/1.4.2/guice-plexus-1.4.2.pom (3.1 kB at 8.9 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/sonatype/sisu/inject/guice-bean/1.4.2/guice-bean-1.4.2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/sonatype/sisu/inject/guice-bean/1.4.2/guice-bean-1.4.2.pom (2.6 kB at 7.5 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/sonatype/sisu/sisu-inject/1.4.2/sisu-inject-1.4.2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/sonatype/sisu/sisu-inject/1.4.2/sisu-inject-1.4.2.pom (1.2 kB at 3.6 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/sonatype/sisu/sisu-parent/1.4.2/sisu-parent-1.4.2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/sonatype/sisu/sisu-parent/1.4.2/sisu-parent-1.4.2.pom (7.8 kB at 22 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/sonatype/forge/forge-parent/6/forge-parent-6.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/sonatype/forge/forge-parent/6/forge-parent-6.pom (11 kB at 31 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-component-annotations/1.7.1/plexus-component-annotations-1.7.1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-component-annotations/1.7.1/plexus-component-annotations-1.7.1.pom (770 B at 2.1 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-containers/1.7.1/plexus-containers-1.7.1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-containers/1.7.1/plexus-containers-1.7.1.pom (5.0 kB at 14 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus/4.0/plexus-4.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus/4.0/plexus-4.0.pom (22 kB at 61 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-classworlds/2.2.3/plexus-classworlds-2.2.3.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-classworlds/2.2.3/plexus-classworlds-2.2.3.pom (4.0 kB at 11 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-utils/2.0.5/plexus-utils-2.0.5.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-utils/2.0.5/plexus-utils-2.0.5.pom (3.3 kB at 9.5 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/sonatype/sisu/sisu-inject-bean/1.4.2/sisu-inject-bean-1.4.2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/sonatype/sisu/sisu-inject-bean/1.4.2/sisu-inject-bean-1.4.2.pom (5.5 kB at 16 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/sonatype/sisu/sisu-guice/2.1.7/sisu-guice-2.1.7.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/sonatype/sisu/sisu-guice/2.1.7/sisu-guice-2.1.7.pom (11 kB at 31 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-core/3.0/maven-core-3.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-core/3.0/maven-core-3.0.pom (6.6 kB at 19 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-settings/3.0/maven-settings-3.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-settings/3.0/maven-settings-3.0.pom (1.9 kB at 5.4 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-settings-builder/3.0/maven-settings-builder-3.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-settings-builder/3.0/maven-settings-builder-3.0.pom (2.2 kB at 6.3 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-interpolation/1.14/plexus-interpolation-1.14.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-interpolation/1.14/plexus-interpolation-1.14.pom (910 B at 2.6 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-components/1.1.18/plexus-components-1.1.18.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-components/1.1.18/plexus-components-1.1.18.pom (5.4 kB at 15 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-repository-metadata/3.0/maven-repository-metadata-3.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-repository-metadata/3.0/maven-repository-metadata-3.0.pom (1.9 kB at 5.5 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-model-builder/3.0/maven-model-builder-3.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-model-builder/3.0/maven-model-builder-3.0.pom (2.2 kB at 6.4 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-aether-provider/3.0/maven-aether-provider-3.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-aether-provider/3.0/maven-aether-provider-3.0.pom (2.5 kB at 6.1 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/sonatype/aether/aether-api/1.7/aether-api-1.7.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/sonatype/aether/aether-api/1.7/aether-api-1.7.pom (1.7 kB at 4.8 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/sonatype/aether/aether-parent/1.7/aether-parent-1.7.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/sonatype/aether/aether-parent/1.7/aether-parent-1.7.pom (7.7 kB at 22 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/sonatype/aether/aether-util/1.7/aether-util-1.7.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/sonatype/aether/aether-util/1.7/aether-util-1.7.pom (2.1 kB at 5.9 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/sonatype/aether/aether-impl/1.7/aether-impl-1.7.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/sonatype/aether/aether-impl/1.7/aether-impl-1.7.pom (3.7 kB at 11 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/sonatype/aether/aether-spi/1.7/aether-spi-1.7.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/sonatype/aether/aether-spi/1.7/aether-spi-1.7.pom (1.7 kB at 4.8 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-shared-utils/3.2.1/maven-shared-utils-3.2.1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-shared-utils/3.2.1/maven-shared-utils-3.2.1.pom (5.6 kB at 16 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-shared-components/30/maven-shared-components-30.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-shared-components/30/maven-shared-components-30.pom (4.6 kB at 13 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-parent/30/maven-parent-30.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-parent/30/maven-parent-30.pom (41 kB at 114 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/commons-io/commons-io/2.5/commons-io-2.5.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/commons-io/commons-io/2.5/commons-io-2.5.pom (13 kB at 34 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/commons/commons-parent/39/commons-parent-39.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/commons/commons-parent/39/commons-parent-39.pom (62 kB at 174 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/apache/16/apache-16.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/apache/16/apache-16.pom (15 kB at 43 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-shared-incremental/1.1/maven-shared-incremental-1.1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-shared-incremental/1.1/maven-shared-incremental-1.1.pom (4.7 kB at 13 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-shared-components/19/maven-shared-components-19.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-shared-components/19/maven-shared-components-19.pom (6.4 kB at 18 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-plugin-api/2.2.1/maven-plugin-api-2.2.1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-plugin-api/2.2.1/maven-plugin-api-2.2.1.pom (1.5 kB at 4.2 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven/2.2.1/maven-2.2.1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven/2.2.1/maven-2.2.1.pom (22 kB at 63 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-parent/11/maven-parent-11.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-parent/11/maven-parent-11.pom (32 kB at 90 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/apache/5/apache-5.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/apache/5/apache-5.pom (4.1 kB at 12 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-core/2.2.1/maven-core-2.2.1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-core/2.2.1/maven-core-2.2.1.pom (12 kB at 33 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-settings/2.2.1/maven-settings-2.2.1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-settings/2.2.1/maven-settings-2.2.1.pom (2.2 kB at 5.9 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-model/2.2.1/maven-model-2.2.1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-model/2.2.1/maven-model-2.2.1.pom (3.2 kB at 9.3 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-utils/1.5.15/plexus-utils-1.5.15.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-utils/1.5.15/plexus-utils-1.5.15.pom (6.8 kB at 20 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-interpolation/1.11/plexus-interpolation-1.11.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-interpolation/1.11/plexus-interpolation-1.11.pom (889 B at 2.5 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-components/1.1.14/plexus-components-1.1.14.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-components/1.1.14/plexus-components-1.1.14.pom (5.8 kB at 16 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-plugin-parameter-documenter/2.2.1/maven-plugin-parameter-documenter-2.2.1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-plugin-parameter-documenter/2.2.1/maven-plugin-parameter-documenter-2.2.1.pom (2.0 kB at 5.6 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/slf4j/slf4j-jdk14/1.5.6/slf4j-jdk14-1.5.6.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/slf4j/slf4j-jdk14/1.5.6/slf4j-jdk14-1.5.6.pom (1.9 kB at 5.4 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/slf4j/slf4j-parent/1.5.6/slf4j-parent-1.5.6.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/slf4j/slf4j-parent/1.5.6/slf4j-parent-1.5.6.pom (7.9 kB at 23 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/slf4j/slf4j-api/1.5.6/slf4j-api-1.5.6.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/slf4j/slf4j-api/1.5.6/slf4j-api-1.5.6.pom (3.0 kB at 8.6 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/slf4j/jcl-over-slf4j/1.5.6/jcl-over-slf4j-1.5.6.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/slf4j/jcl-over-slf4j/1.5.6/jcl-over-slf4j-1.5.6.pom (2.2 kB at 6.2 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-profile/2.2.1/maven-profile-2.2.1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-profile/2.2.1/maven-profile-2.2.1.pom (2.2 kB at 6.2 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-artifact/2.2.1/maven-artifact-2.2.1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-artifact/2.2.1/maven-artifact-2.2.1.pom (1.6 kB at 4.5 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-repository-metadata/2.2.1/maven-repository-metadata-2.2.1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-repository-metadata/2.2.1/maven-repository-metadata-2.2.1.pom (1.9 kB at 5.3 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-error-diagnostics/2.2.1/maven-error-diagnostics-2.2.1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-error-diagnostics/2.2.1/maven-error-diagnostics-2.2.1.pom (1.7 kB at 4.9 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-project/2.2.1/maven-project-2.2.1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-project/2.2.1/maven-project-2.2.1.pom (2.8 kB at 7.9 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-artifact-manager/2.2.1/maven-artifact-manager-2.2.1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-artifact-manager/2.2.1/maven-artifact-manager-2.2.1.pom (3.1 kB at 8.8 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/backport-util-concurrent/backport-util-concurrent/3.1/backport-util-concurrent-3.1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/backport-util-concurrent/backport-util-concurrent/3.1/backport-util-concurrent-3.1.pom (880 B at 2.5 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-plugin-registry/2.2.1/maven-plugin-registry-2.2.1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-plugin-registry/2.2.1/maven-plugin-registry-2.2.1.pom (1.9 kB at 5.4 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-plugin-descriptor/2.2.1/maven-plugin-descriptor-2.2.1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-plugin-descriptor/2.2.1/maven-plugin-descriptor-2.2.1.pom (2.1 kB at 5.9 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-monitor/2.2.1/maven-monitor-2.2.1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-monitor/2.2.1/maven-monitor-2.2.1.pom (1.3 kB at 3.5 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-shared-utils/0.1/maven-shared-utils-0.1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-shared-utils/0.1/maven-shared-utils-0.1.pom (4.0 kB at 11 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-shared-components/18/maven-shared-components-18.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-shared-components/18/maven-shared-components-18.pom (4.9 kB at 14 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-parent/22/maven-parent-22.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-parent/22/maven-parent-22.pom (30 kB at 84 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/apache/11/apache-11.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/apache/11/apache-11.pom (15 kB at 42 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/com/google/code/findbugs/jsr305/2.0.1/jsr305-2.0.1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/com/google/code/findbugs/jsr305/2.0.1/jsr305-2.0.1.pom (965 B at 2.8 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-java/0.9.10/plexus-java-0.9.10.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-java/0.9.10/plexus-java-0.9.10.pom (5.1 kB at 15 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-languages/0.9.10/plexus-languages-0.9.10.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-languages/0.9.10/plexus-languages-0.9.10.pom (4.1 kB at 12 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/ow2/asm/asm/6.2/asm-6.2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/ow2/asm/asm/6.2/asm-6.2.pom (2.9 kB at 8.4 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/ow2/ow2/1.5/ow2-1.5.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/ow2/ow2/1.5/ow2-1.5.pom (11 kB at 32 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/com/thoughtworks/qdox/qdox/2.0-M9/qdox-2.0-M9.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/com/thoughtworks/qdox/qdox/2.0-M9/qdox-2.0-M9.pom (16 kB at 45 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/sonatype/oss/oss-parent/9/oss-parent-9.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/sonatype/oss/oss-parent/9/oss-parent-9.pom (6.6 kB at 19 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-compiler-api/2.8.4/plexus-compiler-api-2.8.4.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-compiler-api/2.8.4/plexus-compiler-api-2.8.4.pom (867 B at 2.5 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-compiler/2.8.4/plexus-compiler-2.8.4.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-compiler/2.8.4/plexus-compiler-2.8.4.pom (6.0 kB at 17 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-components/4.0/plexus-components-4.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-components/4.0/plexus-components-4.0.pom (2.7 kB at 7.6 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-utils/3.0.22/plexus-utils-3.0.22.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-utils/3.0.22/plexus-utils-3.0.22.pom (3.8 kB at 11 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus/3.3.1/plexus-3.3.1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus/3.3.1/plexus-3.3.1.pom (20 kB at 58 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-compiler-manager/2.8.4/plexus-compiler-manager-2.8.4.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-compiler-manager/2.8.4/plexus-compiler-manager-2.8.4.pom (692 B at 2.0 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-compiler-javac/2.8.4/plexus-compiler-javac-2.8.4.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-compiler-javac/2.8.4/plexus-compiler-javac-2.8.4.pom (771 B at 2.2 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-compilers/2.8.4/plexus-compilers-2.8.4.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-compilers/2.8.4/plexus-compilers-2.8.4.pom (1.3 kB at 3.8 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-plugin-api/3.0/maven-plugin-api-3.0.jar
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/sonatype/sisu/sisu-guice/2.1.7/sisu-guice-2.1.7-noaop.jar
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/sonatype/sisu/sisu-inject-plexus/1.4.2/sisu-inject-plexus-1.4.2.jar
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-model/3.0/maven-model-3.0.jar
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/sonatype/sisu/sisu-inject-bean/1.4.2/sisu-inject-bean-1.4.2.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-plugin-api/3.0/maven-plugin-api-3.0.jar (49 kB at 133 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-artifact/3.0/maven-artifact-3.0.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-model/3.0/maven-model-3.0.jar (165 kB at 413 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-utils/2.0.4/plexus-utils-2.0.4.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/sonatype/sisu/sisu-inject-bean/1.4.2/sisu-inject-bean-1.4.2.jar (153 kB at 383 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-core/3.0/maven-core-3.0.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/sonatype/sisu/sisu-guice/2.1.7/sisu-guice-2.1.7-noaop.jar (472 kB at 1.1 MB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-settings/3.0/maven-settings-3.0.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/sonatype/sisu/sisu-inject-plexus/1.4.2/sisu-inject-plexus-1.4.2.jar (202 kB at 380 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-settings-builder/3.0/maven-settings-builder-3.0.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-artifact/3.0/maven-artifact-3.0.jar (52 kB at 72 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-repository-metadata/3.0/maven-repository-metadata-3.0.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-utils/2.0.4/plexus-utils-2.0.4.jar (222 kB at 274 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-model-builder/3.0/maven-model-builder-3.0.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-settings/3.0/maven-settings-3.0.jar (47 kB at 57 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-aether-provider/3.0/maven-aether-provider-3.0.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-core/3.0/maven-core-3.0.jar (527 kB at 645 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/sonatype/aether/aether-impl/1.7/aether-impl-1.7.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-settings-builder/3.0/maven-settings-builder-3.0.jar (38 kB at 41 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/sonatype/aether/aether-spi/1.7/aether-spi-1.7.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-repository-metadata/3.0/maven-repository-metadata-3.0.jar (30 kB at 28 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/sonatype/aether/aether-api/1.7/aether-api-1.7.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-aether-provider/3.0/maven-aether-provider-3.0.jar (51 kB at 43 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/sonatype/aether/aether-util/1.7/aether-util-1.7.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-model-builder/3.0/maven-model-builder-3.0.jar (148 kB at 125 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-interpolation/1.14/plexus-interpolation-1.14.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/sonatype/aether/aether-impl/1.7/aether-impl-1.7.jar (106 kB at 89 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-classworlds/2.2.3/plexus-classworlds-2.2.3.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/sonatype/aether/aether-spi/1.7/aether-spi-1.7.jar (14 kB at 10 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-component-annotations/1.7.1/plexus-component-annotations-1.7.1.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/sonatype/aether/aether-api/1.7/aether-api-1.7.jar (74 kB at 52 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-shared-utils/3.2.1/maven-shared-utils-3.2.1.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-interpolation/1.14/plexus-interpolation-1.14.jar (61 kB at 39 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/commons-io/commons-io/2.5/commons-io-2.5.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/sonatype/aether/aether-util/1.7/aether-util-1.7.jar (108 kB at 69 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-shared-incremental/1.1/maven-shared-incremental-1.1.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-classworlds/2.2.3/plexus-classworlds-2.2.3.jar (46 kB at 30 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-java/0.9.10/plexus-java-0.9.10.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-component-annotations/1.7.1/plexus-component-annotations-1.7.1.jar (4.3 kB at 2.6 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/ow2/asm/asm/6.2/asm-6.2.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-shared-utils/3.2.1/maven-shared-utils-3.2.1.jar (167 kB at 92 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/com/thoughtworks/qdox/qdox/2.0-M9/qdox-2.0-M9.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/commons-io/commons-io/2.5/commons-io-2.5.jar (209 kB at 109 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-compiler-api/2.8.4/plexus-compiler-api-2.8.4.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-shared-incremental/1.1/maven-shared-incremental-1.1.jar (14 kB at 7.0 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-compiler-manager/2.8.4/plexus-compiler-manager-2.8.4.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-java/0.9.10/plexus-java-0.9.10.jar (39 kB at 21 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-compiler-javac/2.8.4/plexus-compiler-javac-2.8.4.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/ow2/asm/asm/6.2/asm-6.2.jar (111 kB at 53 kB/s)
[INFO] Downloaded from central: https://repo1.maven.org/maven2/com/thoughtworks/qdox/qdox/2.0-M9/qdox-2.0-M9.jar (317 kB at 145 kB/s)
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-compiler-manager/2.8.4/plexus-compiler-manager-2.8.4.jar (4.7 kB at 2.1 kB/s)
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-compiler-javac/2.8.4/plexus-compiler-javac-2.8.4.jar (21 kB at 9.3 kB/s)
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-compiler-api/2.8.4/plexus-compiler-api-2.8.4.jar (27 kB at 12 kB/s)
[INFO] Changes detected - recompiling the module!
[INFO] Compiling 2 source files to /tmp/src/target/classes
[INFO] 
[INFO] --- maven-resources-plugin:3.2.0:testResources (default-testResources) @ spring-hello ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] Using 'UTF-8' encoding to copy filtered properties files.
[INFO] skip non existing resourceDirectory /tmp/src/src/test/resources
[INFO] 
[INFO] --- maven-compiler-plugin:3.8.1:testCompile (default-testCompile) @ spring-hello ---
[INFO] No sources to compile
[INFO] 
[INFO] --- maven-surefire-plugin:2.22.2:test (default-test) @ spring-hello ---
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/surefire/maven-surefire-common/2.22.2/maven-surefire-common-2.22.2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/surefire/maven-surefire-common/2.22.2/maven-surefire-common-2.22.2.pom (11 kB at 33 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/plugin-tools/maven-plugin-annotations/3.5.2/maven-plugin-annotations-3.5.2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/plugin-tools/maven-plugin-annotations/3.5.2/maven-plugin-annotations-3.5.2.pom (1.6 kB at 4.6 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/plugin-tools/maven-plugin-tools/3.5.2/maven-plugin-tools-3.5.2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/plugin-tools/maven-plugin-tools/3.5.2/maven-plugin-tools-3.5.2.pom (15 kB at 44 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-parent/31/maven-parent-31.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-parent/31/maven-parent-31.pom (43 kB at 121 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/surefire/surefire-api/2.22.2/surefire-api-2.22.2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/surefire/surefire-api/2.22.2/surefire-api-2.22.2.pom (3.5 kB at 9.9 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/surefire/surefire-logger-api/2.22.2/surefire-logger-api-2.22.2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/surefire/surefire-logger-api/2.22.2/surefire-logger-api-2.22.2.pom (2.0 kB at 5.2 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/surefire/surefire-booter/2.22.2/surefire-booter-2.22.2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/surefire/surefire-booter/2.22.2/surefire-booter-2.22.2.pom (7.5 kB at 20 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-container-default/1.0-alpha-9-stable-1/plexus-container-default-1.0-alpha-9-stable-1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-container-default/1.0-alpha-9-stable-1/plexus-container-default-1.0-alpha-9-stable-1.pom (3.9 kB at 11 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-containers/1.0.3/plexus-containers-1.0.3.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-containers/1.0.3/plexus-containers-1.0.3.pom (492 B at 1.4 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus/1.0.4/plexus-1.0.4.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus/1.0.4/plexus-1.0.4.pom (5.7 kB at 7.8 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/junit/junit/4.12/junit-4.12.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/junit/junit/4.12/junit-4.12.pom (24 kB at 59 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/hamcrest/hamcrest-core/1.3/hamcrest-core-1.3.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/hamcrest/hamcrest-core/1.3/hamcrest-core-1.3.pom (766 B at 2.2 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/hamcrest/hamcrest-parent/1.3/hamcrest-parent-1.3.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/hamcrest/hamcrest-parent/1.3/hamcrest-parent-1.3.pom (2.0 kB at 5.6 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-utils/1.0.4/plexus-utils-1.0.4.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-utils/1.0.4/plexus-utils-1.0.4.pom (6.9 kB at 19 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/classworlds/classworlds/1.1-alpha-2/classworlds-1.1-alpha-2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/classworlds/classworlds/1.1-alpha-2/classworlds-1.1-alpha-2.pom (3.1 kB at 8.9 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/reporting/maven-reporting-api/3.0/maven-reporting-api-3.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/reporting/maven-reporting-api/3.0/maven-reporting-api-3.0.pom (2.4 kB at 6.6 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-shared-components/15/maven-shared-components-15.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-shared-components/15/maven-shared-components-15.pom (9.3 kB at 26 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-parent/16/maven-parent-16.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-parent/16/maven-parent-16.pom (23 kB at 66 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/apache/7/apache-7.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/apache/7/apache-7.pom (14 kB at 41 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/classworlds/classworlds/1.1/classworlds-1.1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/classworlds/classworlds/1.1/classworlds-1.1.pom (3.3 kB at 9.5 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-toolchain/2.2.1/maven-toolchain-2.2.1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-toolchain/2.2.1/maven-toolchain-2.2.1.pom (3.3 kB at 9.5 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/com/thoughtworks/qdox/qdox/2.0-M8/qdox-2.0-M8.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/com/thoughtworks/qdox/qdox/2.0-M8/qdox-2.0-M8.pom (16 kB at 45 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/surefire/maven-surefire-common/2.22.2/maven-surefire-common-2.22.2.jar
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/surefire/surefire-logger-api/2.22.2/surefire-logger-api-2.22.2.jar
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-plugin-api/2.2.1/maven-plugin-api-2.2.1.jar
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/surefire/surefire-api/2.22.2/surefire-api-2.22.2.jar
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/plugin-tools/maven-plugin-annotations/3.5.2/maven-plugin-annotations-3.5.2.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/surefire/surefire-logger-api/2.22.2/surefire-logger-api-2.22.2.jar (13 kB at 37 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/surefire/surefire-booter/2.22.2/surefire-booter-2.22.2.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-plugin-api/2.2.1/maven-plugin-api-2.2.1.jar (12 kB at 33 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-artifact/2.2.1/maven-artifact-2.2.1.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/surefire/surefire-api/2.22.2/surefire-api-2.22.2.jar (186 kB at 474 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-utils/1.5.15/plexus-utils-1.5.15.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/plugin-tools/maven-plugin-annotations/3.5.2/maven-plugin-annotations-3.5.2.jar (14 kB at 35 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-plugin-descriptor/2.2.1/maven-plugin-descriptor-2.2.1.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/surefire/maven-surefire-common/2.22.2/maven-surefire-common-2.22.2.jar (528 kB at 1.2 MB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-container-default/1.0-alpha-9-stable-1/plexus-container-default-1.0-alpha-9-stable-1.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/surefire/surefire-booter/2.22.2/surefire-booter-2.22.2.jar (274 kB at 371 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/junit/junit/4.12/junit-4.12.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-artifact/2.2.1/maven-artifact-2.2.1.jar (80 kB at 108 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/hamcrest/hamcrest-core/1.3/hamcrest-core-1.3.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-utils/1.5.15/plexus-utils-1.5.15.jar (228 kB at 298 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-project/2.2.1/maven-project-2.2.1.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-container-default/1.0-alpha-9-stable-1/plexus-container-default-1.0-alpha-9-stable-1.jar (194 kB at 244 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-settings/2.2.1/maven-settings-2.2.1.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-plugin-descriptor/2.2.1/maven-plugin-descriptor-2.2.1.jar (39 kB at 49 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-profile/2.2.1/maven-profile-2.2.1.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/hamcrest/hamcrest-core/1.3/hamcrest-core-1.3.jar (45 kB at 41 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-artifact-manager/2.2.1/maven-artifact-manager-2.2.1.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/junit/junit/4.12/junit-4.12.jar (315 kB at 276 kB/s)
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-project/2.2.1/maven-project-2.2.1.jar (156 kB at 137 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/backport-util-concurrent/backport-util-concurrent/3.1/backport-util-concurrent-3.1.jar
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-plugin-registry/2.2.1/maven-plugin-registry-2.2.1.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-settings/2.2.1/maven-settings-2.2.1.jar (49 kB at 43 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-interpolation/1.11/plexus-interpolation-1.11.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-profile/2.2.1/maven-profile-2.2.1.jar (35 kB at 30 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-model/2.2.1/maven-model-2.2.1.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-artifact-manager/2.2.1/maven-artifact-manager-2.2.1.jar (68 kB at 47 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-core/2.2.1/maven-core-2.2.1.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-plugin-registry/2.2.1/maven-plugin-registry-2.2.1.jar (30 kB at 20 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-plugin-parameter-documenter/2.2.1/maven-plugin-parameter-documenter-2.2.1.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-interpolation/1.11/plexus-interpolation-1.11.jar (51 kB at 34 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/slf4j/slf4j-jdk14/1.5.6/slf4j-jdk14-1.5.6.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/backport-util-concurrent/backport-util-concurrent/3.1/backport-util-concurrent-3.1.jar (332 kB at 214 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/slf4j/slf4j-api/1.5.6/slf4j-api-1.5.6.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-model/2.2.1/maven-model-2.2.1.jar (88 kB at 54 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/slf4j/jcl-over-slf4j/1.5.6/jcl-over-slf4j-1.5.6.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-core/2.2.1/maven-core-2.2.1.jar (178 kB at 98 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/reporting/maven-reporting-api/3.0/maven-reporting-api-3.0.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-plugin-parameter-documenter/2.2.1/maven-plugin-parameter-documenter-2.2.1.jar (22 kB at 12 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-repository-metadata/2.2.1/maven-repository-metadata-2.2.1.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/slf4j/slf4j-jdk14/1.5.6/slf4j-jdk14-1.5.6.jar (8.8 kB at 4.7 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-error-diagnostics/2.2.1/maven-error-diagnostics-2.2.1.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/slf4j/slf4j-api/1.5.6/slf4j-api-1.5.6.jar (22 kB at 12 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-monitor/2.2.1/maven-monitor-2.2.1.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/slf4j/jcl-over-slf4j/1.5.6/jcl-over-slf4j-1.5.6.jar (17 kB at 8.4 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/classworlds/classworlds/1.1/classworlds-1.1.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/reporting/maven-reporting-api/3.0/maven-reporting-api-3.0.jar (11 kB at 5.0 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-toolchain/2.2.1/maven-toolchain-2.2.1.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-repository-metadata/2.2.1/maven-repository-metadata-2.2.1.jar (26 kB at 12 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/com/thoughtworks/qdox/qdox/2.0-M8/qdox-2.0-M8.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-error-diagnostics/2.2.1/maven-error-diagnostics-2.2.1.jar (13 kB at 5.9 kB/s)
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-monitor/2.2.1/maven-monitor-2.2.1.jar (10 kB at 4.6 kB/s)
[INFO] Downloaded from central: https://repo1.maven.org/maven2/classworlds/classworlds/1.1/classworlds-1.1.jar (38 kB at 16 kB/s)
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-toolchain/2.2.1/maven-toolchain-2.2.1.jar (38 kB at 15 kB/s)
[INFO] Downloaded from central: https://repo1.maven.org/maven2/com/thoughtworks/qdox/qdox/2.0-M8/qdox-2.0-M8.jar (316 kB at 122 kB/s)
[INFO] Tests are skipped.
[INFO] 
[INFO] --- maven-jar-plugin:3.2.0:jar (default-jar) @ spring-hello ---
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/shared/file-management/3.0.0/file-management-3.0.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/shared/file-management/3.0.0/file-management-3.0.0.pom (4.7 kB at 13 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-shared-components/22/maven-shared-components-22.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-shared-components/22/maven-shared-components-22.pom (5.1 kB at 14 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-parent/27/maven-parent-27.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-parent/27/maven-parent-27.pom (41 kB at 104 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/apache/17/apache-17.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/apache/17/apache-17.pom (16 kB at 43 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-shared-io/3.0.0/maven-shared-io-3.0.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-shared-io/3.0.0/maven-shared-io-3.0.0.pom (4.2 kB at 11 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-compat/3.0/maven-compat-3.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-compat/3.0/maven-compat-3.0.pom (4.0 kB at 11 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/wagon/wagon-provider-api/1.0-beta-6/wagon-provider-api-1.0-beta-6.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/wagon/wagon-provider-api/1.0-beta-6/wagon-provider-api-1.0-beta-6.pom (1.8 kB at 4.8 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/wagon/wagon/1.0-beta-6/wagon-1.0-beta-6.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/wagon/wagon/1.0-beta-6/wagon-1.0-beta-6.pom (12 kB at 33 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-utils/1.4.2/plexus-utils-1.4.2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-utils/1.4.2/plexus-utils-1.4.2.pom (2.0 kB at 4.0 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/wagon/wagon-provider-api/2.10/wagon-provider-api-2.10.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/wagon/wagon-provider-api/2.10/wagon-provider-api-2.10.pom (1.7 kB at 4.8 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/wagon/wagon/2.10/wagon-2.10.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/wagon/wagon/2.10/wagon-2.10.pom (21 kB at 54 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-parent/26/maven-parent-26.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-parent/26/maven-parent-26.pom (40 kB at 100 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-utils/3.0.15/plexus-utils-3.0.15.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-utils/3.0.15/plexus-utils-3.0.15.pom (3.1 kB at 8.6 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-shared-utils/3.0.0/maven-shared-utils-3.0.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-shared-utils/3.0.0/maven-shared-utils-3.0.0.pom (5.6 kB at 15 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-shared-components/21/maven-shared-components-21.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-shared-components/21/maven-shared-components-21.pom (5.1 kB at 14 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-parent/25/maven-parent-25.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-parent/25/maven-parent-25.pom (37 kB at 96 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/apache/15/apache-15.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/apache/15/apache-15.pom (15 kB at 40 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/commons-io/commons-io/2.4/commons-io-2.4.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/commons-io/commons-io/2.4/commons-io-2.4.pom (10 kB at 26 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/commons/commons-parent/25/commons-parent-25.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/commons/commons-parent/25/commons-parent-25.pom (48 kB at 121 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/apache/9/apache-9.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/apache/9/apache-9.pom (15 kB at 41 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-archiver/3.5.0/maven-archiver-3.5.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-archiver/3.5.0/maven-archiver-3.5.0.pom (4.5 kB at 12 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-shared-components/33/maven-shared-components-33.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-shared-components/33/maven-shared-components-33.pom (5.1 kB at 14 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-archiver/4.2.0/plexus-archiver-4.2.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-archiver/4.2.0/plexus-archiver-4.2.0.pom (4.8 kB at 13 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-io/3.2.0/plexus-io-3.2.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-io/3.2.0/plexus-io-3.2.0.pom (4.5 kB at 12 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/commons/commons-compress/1.19/commons-compress-1.19.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/commons/commons-compress/1.19/commons-compress-1.19.pom (18 kB at 48 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/commons/commons-parent/48/commons-parent-48.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/commons/commons-parent/48/commons-parent-48.pom (72 kB at 174 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/iq80/snappy/snappy/0.4/snappy-0.4.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/iq80/snappy/snappy/0.4/snappy-0.4.pom (15 kB at 39 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/tukaani/xz/1.8/xz-1.8.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/tukaani/xz/1.8/xz-1.8.pom (1.9 kB at 5.2 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-interpolation/1.25/plexus-interpolation-1.25.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-interpolation/1.25/plexus-interpolation-1.25.pom (2.6 kB at 7.2 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-archiver/4.2.1/plexus-archiver-4.2.1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-archiver/4.2.1/plexus-archiver-4.2.1.pom (4.8 kB at 13 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/shared/file-management/3.0.0/file-management-3.0.0.jar
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-compat/3.0/maven-compat-3.0.jar
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-shared-io/3.0.0/maven-shared-io-3.0.0.jar
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-archiver/3.5.0/maven-archiver-3.5.0.jar
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/wagon/wagon-provider-api/2.10/wagon-provider-api-2.10.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-shared-io/3.0.0/maven-shared-io-3.0.0.jar (41 kB at 112 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-archiver/4.2.1/plexus-archiver-4.2.1.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-archiver/3.5.0/maven-archiver-3.5.0.jar (26 kB at 68 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-io/3.2.0/plexus-io-3.2.0.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-compat/3.0/maven-compat-3.0.jar (285 kB at 736 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/commons/commons-compress/1.19/commons-compress-1.19.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/shared/file-management/3.0.0/file-management-3.0.0.jar (35 kB at 89 kB/s)
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/wagon/wagon-provider-api/2.10/wagon-provider-api-2.10.jar (54 kB at 134 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/iq80/snappy/snappy/0.4/snappy-0.4.jar
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/tukaani/xz/1.8/xz-1.8.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-archiver/4.2.1/plexus-archiver-4.2.1.jar (196 kB at 263 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-utils/3.3.0/plexus-utils-3.3.0.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-io/3.2.0/plexus-io-3.2.0.jar (76 kB at 99 kB/s)
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/iq80/snappy/snappy/0.4/snappy-0.4.jar (58 kB at 75 kB/s)
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/commons/commons-compress/1.19/commons-compress-1.19.jar (615 kB at 769 kB/s)
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/tukaani/xz/1.8/xz-1.8.jar (109 kB at 128 kB/s)
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-utils/3.3.0/plexus-utils-3.3.0.jar (263 kB at 235 kB/s)
[INFO] Building jar: /tmp/src/target/spring-hello-1.3.jar
[INFO] 
[INFO] --- spring-boot-maven-plugin:2.4.2:repackage (repackage) @ spring-hello ---
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-buildpack-platform/2.4.2/spring-boot-buildpack-platform-2.4.2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-buildpack-platform/2.4.2/spring-boot-buildpack-platform-2.4.2.pom (3.1 kB at 8.6 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/net/java/dev/jna/jna-platform/5.5.0/jna-platform-5.5.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/net/java/dev/jna/jna-platform/5.5.0/jna-platform-5.5.0.pom (1.8 kB at 4.2 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/net/java/dev/jna/jna/5.5.0/jna-5.5.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/net/java/dev/jna/jna/5.5.0/jna-5.5.0.pom (1.6 kB at 4.5 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/commons/commons-compress/1.20/commons-compress-1.20.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/commons/commons-compress/1.20/commons-compress-1.20.pom (18 kB at 52 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/httpcomponents/httpclient/4.5.13/httpclient-4.5.13.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/httpcomponents/httpclient/4.5.13/httpclient-4.5.13.pom (6.6 kB at 19 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/httpcomponents/httpcomponents-client/4.5.13/httpcomponents-client-4.5.13.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/httpcomponents/httpcomponents-client/4.5.13/httpcomponents-client-4.5.13.pom (16 kB at 46 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/httpcomponents/httpcomponents-parent/11/httpcomponents-parent-11.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/httpcomponents/httpcomponents-parent/11/httpcomponents-parent-11.pom (35 kB at 97 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/httpcomponents/httpcore/4.4.13/httpcore-4.4.13.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/httpcomponents/httpcore/4.4.13/httpcore-4.4.13.pom (5.0 kB at 14 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/httpcomponents/httpcomponents-core/4.4.13/httpcomponents-core-4.4.13.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/httpcomponents/httpcomponents-core/4.4.13/httpcomponents-core-4.4.13.pom (13 kB at 36 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/commons-logging/commons-logging/1.2/commons-logging-1.2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/commons-logging/commons-logging/1.2/commons-logging-1.2.pom (19 kB at 54 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/commons/commons-parent/34/commons-parent-34.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/commons/commons-parent/34/commons-parent-34.pom (56 kB at 155 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/commons-codec/commons-codec/1.11/commons-codec-1.11.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/commons-codec/commons-codec/1.11/commons-codec-1.11.pom (14 kB at 40 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-loader-tools/2.4.2/spring-boot-loader-tools-2.4.2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-loader-tools/2.4.2/spring-boot-loader-tools-2.4.2.pom (2.3 kB at 6.3 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-common-artifact-filters/3.1.0/maven-common-artifact-filters-3.1.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-common-artifact-filters/3.1.0/maven-common-artifact-filters-3.1.0.pom (5.3 kB at 15 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-component-annotations/1.5.5/plexus-component-annotations-1.5.5.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-component-annotations/1.5.5/plexus-component-annotations-1.5.5.pom (815 B at 2.3 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-containers/1.5.5/plexus-containers-1.5.5.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-containers/1.5.5/plexus-containers-1.5.5.pom (4.2 kB at 12 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-component-annotations/1.5.4/plexus-component-annotations-1.5.4.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-component-annotations/1.5.4/plexus-component-annotations-1.5.4.pom (815 B at 2.3 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-containers/1.5.4/plexus-containers-1.5.4.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-containers/1.5.4/plexus-containers-1.5.4.pom (4.2 kB at 12 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus/2.0.5/plexus-2.0.5.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus/2.0.5/plexus-2.0.5.pom (17 kB at 49 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-shared-utils/3.1.0/maven-shared-utils-3.1.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-shared-utils/3.1.0/maven-shared-utils-3.1.0.pom (5.0 kB at 6.8 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-plugin-api/3.6.3/maven-plugin-api-3.6.3.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-plugin-api/3.6.3/maven-plugin-api-3.6.3.pom (3.0 kB at 4.2 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven/3.6.3/maven-3.6.3.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven/3.6.3/maven-3.6.3.pom (26 kB at 73 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-model/3.6.3/maven-model-3.6.3.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-model/3.6.3/maven-model-3.6.3.pom (4.1 kB at 12 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-utils/3.2.1/plexus-utils-3.2.1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-utils/3.2.1/plexus-utils-3.2.1.pom (5.3 kB at 15 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-artifact/3.6.3/maven-artifact-3.6.3.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-artifact/3.6.3/maven-artifact-3.6.3.pom (2.4 kB at 6.8 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/eclipse/sisu/org.eclipse.sisu.plexus/0.3.4/org.eclipse.sisu.plexus-0.3.4.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/eclipse/sisu/org.eclipse.sisu.plexus/0.3.4/org.eclipse.sisu.plexus-0.3.4.pom (4.2 kB at 12 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/eclipse/sisu/sisu-plexus/0.3.4/sisu-plexus-0.3.4.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/eclipse/sisu/sisu-plexus/0.3.4/sisu-plexus-0.3.4.pom (14 kB at 39 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/eclipse/sisu/org.eclipse.sisu.inject/0.3.4/org.eclipse.sisu.inject-0.3.4.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/eclipse/sisu/org.eclipse.sisu.inject/0.3.4/org.eclipse.sisu.inject-0.3.4.pom (2.6 kB at 6.6 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/eclipse/sisu/sisu-inject/0.3.4/sisu-inject-0.3.4.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/eclipse/sisu/sisu-inject/0.3.4/sisu-inject-0.3.4.pom (14 kB at 41 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-classworlds/2.5.2/plexus-classworlds-2.5.2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-classworlds/2.5.2/plexus-classworlds-2.5.2.pom (7.3 kB at 19 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-utils/3.0.17/plexus-utils-3.0.17.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-utils/3.0.17/plexus-utils-3.0.17.pom (3.4 kB at 9.6 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-classworlds/2.6.0/plexus-classworlds-2.6.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-classworlds/2.6.0/plexus-classworlds-2.6.0.pom (7.9 kB at 22 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-buildpack-platform/2.4.2/spring-boot-buildpack-platform-2.4.2.jar
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/commons/commons-compress/1.20/commons-compress-1.20.jar
[INFO] Downloading from central: https://repo1.maven.org/maven2/net/java/dev/jna/jna-platform/5.5.0/jna-platform-5.5.0.jar
[INFO] Downloading from central: https://repo1.maven.org/maven2/net/java/dev/jna/jna/5.5.0/jna-5.5.0.jar
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/httpcomponents/httpclient/4.5.13/httpclient-4.5.13.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-buildpack-platform/2.4.2/spring-boot-buildpack-platform-2.4.2.jar (202 kB at 346 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/httpcomponents/httpcore/4.4.13/httpcore-4.4.13.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/commons/commons-compress/1.20/commons-compress-1.20.jar (632 kB at 1.1 MB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/commons-logging/commons-logging/1.2/commons-logging-1.2.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/httpcomponents/httpclient/4.5.13/httpclient-4.5.13.jar (780 kB at 1.1 MB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/commons-codec/commons-codec/1.11/commons-codec-1.11.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/net/java/dev/jna/jna-platform/5.5.0/jna-platform-5.5.0.jar (2.7 MB at 3.5 MB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-loader-tools/2.4.2/spring-boot-loader-tools-2.4.2.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/commons-logging/commons-logging/1.2/commons-logging-1.2.jar (62 kB at 64 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-common-artifact-filters/3.1.0/maven-common-artifact-filters-3.1.0.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/httpcomponents/httpcore/4.4.13/httpcore-4.4.13.jar (329 kB at 338 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-component-annotations/1.5.5/plexus-component-annotations-1.5.5.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-loader-tools/2.4.2/spring-boot-loader-tools-2.4.2.jar (243 kB at 213 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-shared-utils/3.1.0/maven-shared-utils-3.1.0.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/commons-codec/commons-codec/1.11/commons-codec-1.11.jar (335 kB at 278 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-plugin-api/3.6.3/maven-plugin-api-3.6.3.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-common-artifact-filters/3.1.0/maven-common-artifact-filters-3.1.0.jar (61 kB at 46 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/eclipse/sisu/org.eclipse.sisu.plexus/0.3.4/org.eclipse.sisu.plexus-0.3.4.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-component-annotations/1.5.5/plexus-component-annotations-1.5.5.jar (4.2 kB at 3.2 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/eclipse/sisu/org.eclipse.sisu.inject/0.3.4/org.eclipse.sisu.inject-0.3.4.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-plugin-api/3.6.3/maven-plugin-api-3.6.3.jar (47 kB at 30 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-utils/3.2.1/plexus-utils-3.2.1.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/eclipse/sisu/org.eclipse.sisu.plexus/0.3.4/org.eclipse.sisu.plexus-0.3.4.jar (205 kB at 121 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-classworlds/2.6.0/plexus-classworlds-2.6.0.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/eclipse/sisu/org.eclipse.sisu.inject/0.3.4/org.eclipse.sisu.inject-0.3.4.jar (379 kB at 218 kB/s)
[INFO] Downloaded from central: https://repo1.maven.org/maven2/net/java/dev/jna/jna/5.5.0/jna-5.5.0.jar (1.5 MB at 840 kB/s)
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-shared-utils/3.1.0/maven-shared-utils-3.1.0.jar (164 kB at 86 kB/s)
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-utils/3.2.1/plexus-utils-3.2.1.jar (262 kB at 132 kB/s)
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-classworlds/2.6.0/plexus-classworlds-2.6.0.jar (53 kB at 26 kB/s)
[INFO] Replacing main artifact with repackaged archive
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  02:39 min
[INFO] Finished at: 2022-11-09T04:46:58Z
[INFO] ------------------------------------------------------------------------
[WARNING] The requested profile "openshift" could not be activated because it does not exist.
INFO Copying deployments from target to /deployments...
'/tmp/src/target/spring-hello-1.3.jar' -> '/deployments/spring-hello-1.3.jar'
INFO Cleaning up source directory (/tmp/src)
STEP 9/9: CMD /usr/local/s2i/run
COMMIT temp.builder.openshift.io/jegan/hello-1:ec43ec32
time="2022-11-09T04:46:58Z" level=warning msg="Adding metacopy option, configured globally"
Getting image source signatures
Copying blob sha256:d90fd9f4a2a3a5970c70b9fbe40a2f95014215221d1f23a5e0a708f5b054135d
Copying blob sha256:0115643dc5be77da42c7be24c5d03da0e78d1f51b429e8d86cee9192528dae5d
Copying blob sha256:769ce20c32a641d61f754cb0ba1b9c5655ba28567f3c167ef4a003e92fa05a3e
Copying config sha256:cfbfd0f65f5ef2a12019b31f3f2e4b2a5941592c7bb69ad9120badff21e9e89c
Writing manifest to image destination
Storing signatures
--> cfbfd0f65f5
Successfully tagged temp.builder.openshift.io/jegan/hello-1:ec43ec32
cfbfd0f65f5ef2a12019b31f3f2e4b2a5941592c7bb69ad9120badff21e9e89c

Pushing image image-registry.openshift-image-registry.svc:5000/jegan/hello:latest ...
Getting image source signatures
Copying blob sha256:488d0614fc378af3bd3dc61a94e61987e86d74a480b8fe0048d8a7e7574202de
Copying blob sha256:550a30ba276ce28ef23d8f3b23ec19faa4173b8c9a197f2331c5df0f8fd9f7b9
Copying blob sha256:769ce20c32a641d61f754cb0ba1b9c5655ba28567f3c167ef4a003e92fa05a3e
Copying config sha256:cfbfd0f65f5ef2a12019b31f3f2e4b2a5941592c7bb69ad9120badff21e9e89c
Writing manifest to image destination
Storing signatures
Successfully pushed image-registry.openshift-image-registry.svc:5000/jegan/hello@sha256:b9a8df1dd0832726f30290d638c367f1971e7413cce7455884cb7f3495c39ea5
Push successful
</pre>

Creating an external route to access your application
```
oc expose svc/hello
oc get route
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>oc get route</b>
NAME    HOST/PORT                           PATH   SERVICES   PORT       TERMINATION   WILDCARD
hello   hello-jegan.apps.ocp.tektutor.org          hello      8080-tcp                 None
(jegan@tektutor.org)$ <b>curl hello-jegan.apps.ocp.tektutor.org</b>
Greetings from Spring Boot!

</pre>

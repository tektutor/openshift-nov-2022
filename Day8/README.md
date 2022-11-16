# Day 8

## Lab - Creating configmap from a file
```
cat temp.properties
oc create configmap test-cm --from-file=./temp.properties
```

Expected output
<pre>
(jegan@tektutor.org)$ cat temp.properties 
language=java
framework=springboot

(jegan@tektutor.org)$ oc create configmap test-cm --from-file=./temp.properties
configmap/test-cm created

jegan@tektutor.org)$ oc describe cm/test-cm
Name:         test-cm
Namespace:    jegan
Labels:       <none>
Annotations:  <none>

Data
====
temp.properties:
----
language=java
framework=springboot


BinaryData
====

Events:  <none>
</pre>

## Lab - Creating ConfigMap from command-line with inline values
```
oc create configmap my-config-map --from-literal=orchestration=openshift --from-literal=container=crio
```

## Lab - Task with ConfigMap mounted as a volume

First create a configmap with bunch of key/value pairs
configmap.yml
```
apiVersion: v1
kind: ConfigMap
metadata:
  name: tools-path-cm
data:
  jdk_home: /usr/lib/jdk11
  m2_home: /usr/share/maven
```

Let's create a configmap using the 


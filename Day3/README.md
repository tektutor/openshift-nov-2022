# Day3

## Lab - Getting inside a Pod using its deployment name
```
oc rsh deploy/nginx
```

## Lab -  Getting inside a specific Pod shell
```
oc rsh pod/
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

# Day 2 ( 4 Hours )

## Listing OpenShift nodes in the cluster
```
oc get nodes
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>oc get nodes</b>
NAME                        STATUS   ROLES           AGE   VERSION
master-1.ocp.tektutor.org   Ready    master,worker   25h   v1.23.12+6b34f32
master-2.ocp.tektutor.org   Ready    master,worker   25h   v1.23.12+6b34f32
master-3.ocp.tektutor.org   Ready    master,worker   25h   v1.23.12+6b34f32
worker-1.ocp.tektutor.org   Ready    worker          25h   v1.23.12+6b34f32
worker-2.ocp.tektutor.org   Ready    worker          25h   v1.23.12+6b34f32
</pre>

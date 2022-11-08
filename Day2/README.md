# Day 2 ( 4 Hours )

## ⛹️‍♂️ Lab - Listing OpenShift nodes in the cluster
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

## Demo - Listing the nodes in Kubernetes cluster
```
kubectl get nodes
```
Expected output
<pre>
jegan@worker1.tektutor.org:~/Desktop$ <b>kubectl get nodes</b>
NAME       STATUS   ROLES           AGE    VERSION
minikube   Ready    control-plane   3m8s   v1.25.3
</pre>


## ⛹️‍♂️ Lab - Listing the projects
```
oc get projects
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>oc get projects</b>
NAME                                               DISPLAY NAME   STATUS
default                                                           Active
kube-node-lease                                                   Active
kube-public                                                       Active
kube-system                                                       Active
openshift                                                         Active
openshift-apiserver                                               Active
openshift-apiserver-operator                                      Active
openshift-authentication                                          Active
openshift-authentication-operator                                 Active
openshift-cloud-controller-manager                                Active
openshift-cloud-controller-manager-operator                       Active
openshift-cloud-credential-operator                               Active
openshift-cloud-network-config-controller                         Active
openshift-cluster-csi-drivers                                     Active
openshift-cluster-machine-approver                                Active
openshift-cluster-node-tuning-operator                            Active
openshift-cluster-samples-operator                                Active
openshift-cluster-storage-operator                                Active
openshift-cluster-version                                         Active
openshift-config                                                  Active
openshift-config-managed                                          Active
openshift-config-operator                                         Active
openshift-console                                                 Active
openshift-console-operator                                        Active
openshift-console-user-settings                                   Active
openshift-controller-manager                                      Active
openshift-controller-manager-operator                             Active
openshift-dns                                                     Active
openshift-dns-operator                                            Active
openshift-etcd                                                    Active
openshift-etcd-operator                                           Active
openshift-host-network                                            Active
openshift-image-registry                                          Active
openshift-infra                                                   Active
openshift-ingress                                                 Active
openshift-ingress-canary                                          Active
openshift-ingress-operator                                        Active
openshift-insights                                                Active
openshift-kni-infra                                               Active
openshift-kube-apiserver                                          Active
openshift-kube-apiserver-operator                                 Active
openshift-kube-controller-manager                                 Active
openshift-kube-controller-manager-operator                        Active
openshift-kube-scheduler                                          Active
openshift-kube-scheduler-operator                                 Active
openshift-kube-storage-version-migrator                           Active
openshift-kube-storage-version-migrator-operator                  Active
openshift-machine-api                                             Active
openshift-machine-config-operator                                 Active
openshift-marketplace                                             Active
openshift-monitoring                                              Active
openshift-multus                                                  Active
openshift-network-diagnostics                                     Active
openshift-network-operator                                        Active
openshift-node                                                    Active
openshift-oauth-apiserver                                         Active
openshift-openstack-infra                                         Active
openshift-operator-lifecycle-manager                              Active
openshift-operators                                               Active
openshift-ovirt-infra                                             Active
openshift-sdn                                                     Active
openshift-service-ca                                              Active
openshift-service-ca-operator                                     Active
openshift-user-workload-monitoring                                Active
openshift-vsphere-infra                                           Active
</pre>

## Demo - Listing the namespaces ( project concept isn't supported in Kubernetes )
```
kubectl get namespaces
```
Expected output
<pre>
jegan@worker1.tektutor.org:~/Desktop$ <b>kubectl get namespaces</b>
NAME              STATUS   AGE
default           Active   11m
kube-node-lease   Active   11m
kube-public       Active   11m
kube-system       Active   11m
</pre>

## ⛹️‍♂️ Lab - Finding the private openshift image registry
```
oc get pods --all-namespaces | grep -i image
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>oc get pods --all-namespaces | grep -i image</b>
openshift-image-registry                           cluster-image-registry-operator-77b89758cf-gqw4c             1/1     Running     0             27h
openshift-image-registry                           image-pruner-27797760-r68zw                                  0/1     Completed   0             6h24m
<b>openshift-image-registry                           image-registry-f884c66b8-htchr                               1/1     Running     0             27h</b>
openshift-image-registry                           node-ca-2jq7q                                                1/1     Running     0             27h
openshift-image-registry                           node-ca-58mzf                                                1/1     Running     0             27h
openshift-image-registry                           node-ca-58ntb                                                1/1     Running     0             27h
openshift-image-registry                           node-ca-b6249                                                1/1     Running     0             27h
openshift-image-registry                           node-ca-qgn98                                                1/1     Running     0             27h
</pre>

## ⛹️‍♂️ Lab - Finding more details about your master node
```
oc describe node master-1.ocp.tektutor.org
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>oc describe node master-1.ocp.tektutor.org</b>
Name:               master-1.ocp.tektutor.org
Roles:              master,worker
Labels:             beta.kubernetes.io/arch=amd64
                    beta.kubernetes.io/os=linux
                    kubernetes.io/arch=amd64
                    kubernetes.io/hostname=master-1.ocp.tektutor.org
                    kubernetes.io/os=linux
                    node-role.kubernetes.io/master=
                    node-role.kubernetes.io/worker=
                    node.openshift.io/os_id=rhcos
Annotations:        machineconfiguration.openshift.io/controlPlaneTopology: HighlyAvailable
                    machineconfiguration.openshift.io/currentConfig: rendered-master-4750d456d0dd04158d1fda17d8ae074e
                    machineconfiguration.openshift.io/desiredConfig: rendered-master-4750d456d0dd04158d1fda17d8ae074e
                    machineconfiguration.openshift.io/reason: 
                    machineconfiguration.openshift.io/state: Done
                    volumes.kubernetes.io/controller-managed-attach-detach: true
CreationTimestamp:  Mon, 07 Nov 2022 08:12:08 +0530
Taints:             <none>
Unschedulable:      false
Lease:
  HolderIdentity:  master-1.ocp.tektutor.org
  AcquireTime:     <unset>
  RenewTime:       Tue, 08 Nov 2022 10:20:30 +0530
Conditions:
  Type             Status  LastHeartbeatTime                 LastTransitionTime                Reason                       Message
  ----             ------  -----------------                 ------------------                ------                       -------
  MemoryPressure   False   Tue, 08 Nov 2022 10:17:08 +0530   Mon, 07 Nov 2022 08:12:08 +0530   KubeletHasSufficientMemory   kubelet has sufficient memory available
  DiskPressure     False   Tue, 08 Nov 2022 10:17:08 +0530   Mon, 07 Nov 2022 08:12:08 +0530   KubeletHasNoDiskPressure     kubelet has no disk pressure
  PIDPressure      False   Tue, 08 Nov 2022 10:17:08 +0530   Mon, 07 Nov 2022 08:12:08 +0530   KubeletHasSufficientPID      kubelet has sufficient PID available
  Ready            True    Tue, 08 Nov 2022 10:17:08 +0530   Mon, 07 Nov 2022 08:13:39 +0530   KubeletReady                 kubelet is posting ready status
Addresses:
  InternalIP:  192.168.122.39
  Hostname:    master-1.ocp.tektutor.org
Capacity:
  cpu:                8
  ephemeral-storage:  104322028Ki
  hugepages-1Gi:      0
  hugepages-2Mi:      0
  memory:             64405380Ki
  pods:               250
Allocatable:
  cpu:                7500m
  ephemeral-storage:  96143180846
  hugepages-1Gi:      0
  hugepages-2Mi:      0
  memory:             63254404Ki
  pods:               250
System Info:
  Machine ID:                                       1c019f9727c444d2bebbdc50e2510d92
  System UUID:                                      1c019f97-27c4-44d2-bebb-dc50e2510d92
  Boot ID:                                          9a187489-8b58-4f75-beb6-8eb7ac46d480
  Kernel Version:                                   4.18.0-305.62.1.el8_4.x86_64
  OS Image:                                         Red Hat Enterprise Linux CoreOS 410.84.202210130022-0 (Ootpa)
  Operating System:                                 linux
  Architecture:                                     amd64
  Container Runtime Version:                        cri-o://1.23.3-17.rhaos4.10.git016b1ca.el8
  Kubelet Version:                                  v1.23.12+6b34f32
  Kube-Proxy Version:                               v1.23.12+6b34f32
Non-terminated Pods:                                (65 in total)
  Namespace                                         Name                                                        CPU Requests  CPU Limits  Memory Requests  Memory Limits  Age
  ---------                                         ----                                                        ------------  ----------  ---------------  -------------  ---
  openshift-apiserver-operator                      openshift-apiserver-operator-6f8cf6c894-xxwrj               10m (0%)      0 (0%)      50Mi (0%)        0 (0%)         26h
  openshift-apiserver                               apiserver-6db64c8c9c-m5lnt                                  110m (1%)     0 (0%)      250Mi (0%)       0 (0%)         25h
  openshift-authentication-operator                 authentication-operator-64d9c4ff7f-wjlh5                    20m (0%)      0 (0%)      200Mi (0%)       0 (0%)         26h
  openshift-authentication                          oauth-openshift-6798f45c64-8zlcn                            10m (0%)      0 (0%)      50Mi (0%)        0 (0%)         25h
  openshift-cloud-credential-operator               cloud-credential-operator-6b7886ff65-mgm8n                  20m (0%)      0 (0%)      170Mi (0%)       0 (0%)         26h
  openshift-cluster-machine-approver                machine-approver-688cbbb568-b8f99                           20m (0%)      0 (0%)      70Mi (0%)        0 (0%)         26h
  openshift-cluster-node-tuning-operator            cluster-node-tuning-operator-89dfbf77f-lspbs                10m (0%)      0 (0%)      20Mi (0%)        0 (0%)         26h
  openshift-cluster-node-tuning-operator            tuned-flf8c                                                 10m (0%)      0 (0%)      50Mi (0%)        0 (0%)         26h
  openshift-cluster-storage-operator                cluster-storage-operator-555878cbb9-f578m                   10m (0%)      0 (0%)      20Mi (0%)        0 (0%)         26h
  openshift-cluster-storage-operator                csi-snapshot-controller-84dc88b7cc-j9b5p                    10m (0%)      0 (0%)      50Mi (0%)        0 (0%)         26h
  openshift-cluster-storage-operator                csi-snapshot-controller-84dc88b7cc-p6c4t                    10m (0%)      0 (0%)      50Mi (0%)        0 (0%)         26h
  openshift-cluster-storage-operator                csi-snapshot-controller-operator-5b7bd4d556-jp4zn           10m (0%)      0 (0%)      65Mi (0%)        0 (0%)         26h
  openshift-cluster-storage-operator                csi-snapshot-webhook-755d5b9b74-498rg                       10m (0%)      0 (0%)      20Mi (0%)        0 (0%)         26h
  openshift-cluster-storage-operator                csi-snapshot-webhook-755d5b9b74-dm2hn                       10m (0%)      0 (0%)      20Mi (0%)        0 (0%)         26h
  openshift-config-operator                         openshift-config-operator-8668864f87-fmgwd                  10m (0%)      0 (0%)      50Mi (0%)        0 (0%)         26h
  openshift-console                                 console-6b95fb8779-sns8q                                    10m (0%)      0 (0%)      100Mi (0%)       0 (0%)         25h
  openshift-controller-manager-operator             openshift-controller-manager-operator-6c944f4b8f-rhbgw      10m (0%)      0 (0%)      50Mi (0%)        0 (0%)         26h
  openshift-controller-manager                      controller-manager-w9s7z                                    100m (1%)     0 (0%)      100Mi (0%)       0 (0%)         142m
  openshift-dns-operator                            dns-operator-6cf9b5fbd6-hjhcc                               20m (0%)      0 (0%)      69Mi (0%)        0 (0%)         26h
  openshift-dns                                     dns-default-d7q9j                                           60m (0%)      0 (0%)      110Mi (0%)       0 (0%)         26h
  openshift-dns                                     node-resolver-tjhxj                                         5m (0%)       0 (0%)      21Mi (0%)        0 (0%)         26h
  openshift-etcd-operator                           etcd-operator-7f5579c777-4kxw8                              10m (0%)      0 (0%)      50Mi (0%)        0 (0%)         26h
  openshift-etcd                                    etcd-master-1.ocp.tektutor.org                              410m (5%)     0 (0%)      980Mi (1%)       0 (0%)         25h
  openshift-etcd                                    etcd-quorum-guard-78f4445675-bpmbm                          10m (0%)      0 (0%)      5Mi (0%)         0 (0%)         26h
  openshift-image-registry                          cluster-image-registry-operator-77b89758cf-gqw4c            10m (0%)      0 (0%)      50Mi (0%)        0 (0%)         26h
  openshift-image-registry                          node-ca-b6249                                               10m (0%)      0 (0%)      10Mi (0%)        0 (0%)         25h
  openshift-ingress-canary                          ingress-canary-46nhh                                        10m (0%)      0 (0%)      20Mi (0%)        0 (0%)         25h
  openshift-ingress-operator                        ingress-operator-6c6957b8ff-r977x                           20m (0%)      0 (0%)      96Mi (0%)        0 (0%)         26h
  openshift-ingress                                 router-default-7f9c4d8749-lq7nk                             100m (1%)     0 (0%)      256Mi (0%)       0 (0%)         25h
  openshift-insights                                insights-operator-9748ddddf-n4lj4                           10m (0%)      0 (0%)      30Mi (0%)        0 (0%)         26h
  openshift-kube-apiserver-operator                 kube-apiserver-operator-7ff465f68-mpxcg                     10m (0%)      0 (0%)      50Mi (0%)        0 (0%)         26h
  openshift-kube-apiserver                          kube-apiserver-guard-master-1.ocp.tektutor.org              10m (0%)      0 (0%)      5Mi (0%)         0 (0%)         25h
  openshift-kube-apiserver                          kube-apiserver-master-1.ocp.tektutor.org                    290m (3%)     0 (0%)      1224Mi (1%)      0 (0%)         134m
  openshift-kube-controller-manager-operator        kube-controller-manager-operator-8445676c7c-srftx           10m (0%)      0 (0%)      50Mi (0%)        0 (0%)         26h
  openshift-kube-controller-manager                 kube-controller-manager-guard-master-1.ocp.tektutor.org     10m (0%)      0 (0%)      5Mi (0%)         0 (0%)         25h
  openshift-kube-controller-manager                 kube-controller-manager-master-1.ocp.tektutor.org           80m (1%)      0 (0%)      500Mi (0%)       0 (0%)         25h
  openshift-kube-scheduler-operator                 openshift-kube-scheduler-operator-745d66d866-gvtl4          10m (0%)      0 (0%)      50Mi (0%)        0 (0%)         26h
  openshift-kube-scheduler                          openshift-kube-scheduler-guard-master-1.ocp.tektutor.org    10m (0%)      0 (0%)      5Mi (0%)         0 (0%)         25h
  openshift-kube-scheduler                          openshift-kube-scheduler-master-1.ocp.tektutor.org          25m (0%)      0 (0%)      150Mi (0%)       0 (0%)         25h
  openshift-kube-storage-version-migrator-operator  kube-storage-version-migrator-operator-6c8f74d994-jjj62     10m (0%)      0 (0%)      50Mi (0%)        0 (0%)         26h
  openshift-kube-storage-version-migrator           migrator-6485dff756-vxvbx                                   10m (0%)      0 (0%)      200Mi (0%)       0 (0%)         26h
  openshift-machine-api                             cluster-autoscaler-operator-7f47b9d79f-2vcg4                30m (0%)      0 (0%)      70Mi (0%)        0 (0%)         26h
  openshift-machine-api                             cluster-baremetal-operator-887799fd8-wxgww                  20m (0%)      0 (0%)      70Mi (0%)        0 (0%)         26h
  openshift-machine-api                             machine-api-operator-69fff8cfbc-tmpz8                       20m (0%)      0 (0%)      70Mi (0%)        0 (0%)         26h
  openshift-machine-config-operator                 machine-config-daemon-jt6nn                                 40m (0%)      0 (0%)      100Mi (0%)       0 (0%)         26h
  openshift-machine-config-operator                 machine-config-operator-84f5985565-vj8kb                    20m (0%)      0 (0%)      50Mi (0%)        0 (0%)         26h
  openshift-machine-config-operator                 machine-config-server-b7p6p                                 20m (0%)      0 (0%)      50Mi (0%)        0 (0%)         26h
  openshift-marketplace                             marketplace-operator-66f6dbb69c-m9gpx                       10m (0%)      0 (0%)      50Mi (0%)        0 (0%)         26h
  openshift-monitoring                              cluster-monitoring-operator-6bc4c5d7fc-p6sr6                11m (0%)      0 (0%)      95Mi (0%)        0 (0%)         26h
  openshift-monitoring                              node-exporter-svl4d                                         9m (0%)       0 (0%)      47Mi (0%)        0 (0%)         26h
  openshift-monitoring                              prometheus-k8s-1                                            100m (1%)     0 (0%)      1104Mi (1%)      0 (0%)         25h
  openshift-monitoring                              thanos-querier-5bbc64c44d-zxz82                             15m (0%)      0 (0%)      92Mi (0%)        0 (0%)         25h
  openshift-multus                                  multus-additional-cni-plugins-nw8x6                         10m (0%)      0 (0%)      10Mi (0%)        0 (0%)         26h
  openshift-multus                                  multus-admission-controller-4vpfr                           20m (0%)      0 (0%)      70Mi (0%)        0 (0%)         26h
  openshift-multus                                  multus-nvds7                                                10m (0%)      0 (0%)      65Mi (0%)        0 (0%)         26h
  openshift-multus                                  network-metrics-daemon-cmdkh                                20m (0%)      0 (0%)      120Mi (0%)       0 (0%)         26h
  openshift-network-diagnostics                     network-check-target-rq2z5                                  10m (0%)      0 (0%)      15Mi (0%)        0 (0%)         26h
  openshift-oauth-apiserver                         apiserver-7c6f97b564-zxfz4                                  150m (2%)     0 (0%)      200Mi (0%)       0 (0%)         25h
  openshift-operator-lifecycle-manager              catalog-operator-c8d84c46c-6m7cv                            10m (0%)      0 (0%)      80Mi (0%)        0 (0%)         26h
  openshift-operator-lifecycle-manager              olm-operator-798d64cc57-kq8xt                               10m (0%)      0 (0%)      160Mi (0%)       0 (0%)         26h
  openshift-operator-lifecycle-manager              package-server-manager-647b48d449-rsdkh                     10m (0%)      0 (0%)      50Mi (0%)        0 (0%)         26h
  openshift-operator-lifecycle-manager              packageserver-5dcd6cccc5-whtn6                              10m (0%)      0 (0%)      50Mi (0%)        0 (0%)         26h
  openshift-sdn                                     sdn-controller-56kfq                                        20m (0%)      0 (0%)      70Mi (0%)        0 (0%)         26h
  openshift-sdn                                     sdn-kkbg8                                                   110m (1%)     0 (0%)      220Mi (0%)       0 (0%)         26h
  openshift-service-ca-operator                     service-ca-operator-6764bf7bd4-twpzq                        10m (0%)      0 (0%)      80Mi (0%)        0 (0%)         26h
Allocated resources:
  (Total limits may be over 100 percent, i.e., overcommitted.)
  Resource           Requests      Limits
  --------           --------      ------
  cpu                2245m (29%)   0 (0%)
  memory             8359Mi (13%)  0 (0%)
  ephemeral-storage  0 (0%)        0 (0%)
  hugepages-1Gi      0 (0%)        0 (0%)
  hugepages-2Mi      0 (0%)        0 (0%)
Events:              <none>
</pre>

## Demo - Listing the Control Plane components in Kubernetes
```
kubectl get po -n kube-system
```

Expected output
<pre>
jegan@worker1.tektutor.org:~/Desktop$ <b>kubectl get po -n kube-system</b>
NAME                               READY   STATUS    RESTARTS   AGE
coredns-565d847f94-zn6s7           1/1     Running   0          3m6s
<b>etcd-minikube                      1/1     Running   0          3m20s
kube-apiserver-minikube            1/1     Running   0          3m18s
kube-controller-manager-minikube   1/1     Running   0          3m18s</b>
kube-proxy-vhnw2                   1/1     Running   0          3m7s
<b>kube-scheduler-minikube            1/1     Running   0          3m18s</b>
storage-provisioner                1/1     Running   0          3m15s
</pre>

## ⛹️‍♂️ Lab - Listing Control Plane components in OpenShift
```
oc get pods -n openshift-apiserver
oc get pods -n openshift-etcd
oc get pods -n openshift-kube-controller-manager
oc get pods -n openshift-kube-scheduler
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>oc get pods -n openshift-apiserver</b>
NAME                         READY   STATUS    RESTARTS   AGE
apiserver-6db64c8c9c-kbc75   2/2     Running   0          26h
apiserver-6db64c8c9c-m5lnt   2/2     Running   0          26h
apiserver-6db64c8c9c-wh8jx   2/2     Running   0          26h
(jegan@tektutor.org)$ <b>oc get pods -n openshift-etcd</b>
NAME                                          READY   STATUS      RESTARTS   AGE
<b>etcd-master-1.ocp.tektutor.org                5/5     Running     0          26h
etcd-master-2.ocp.tektutor.org                5/5     Running     0          26h
etcd-master-3.ocp.tektutor.org                5/5     Running     0          26h</b>
etcd-quorum-guard-78f4445675-bpmbm            1/1     Running     0          26h
etcd-quorum-guard-78f4445675-dzjqk            1/1     Running     0          26h
etcd-quorum-guard-78f4445675-hwms8            1/1     Running     0          26h
installer-5-master-1.ocp.tektutor.org         0/1     Completed   0          26h
installer-5-master-3.ocp.tektutor.org         0/1     Completed   0          26h
installer-7-master-1.ocp.tektutor.org         0/1     Completed   0          26h
installer-7-master-2.ocp.tektutor.org         0/1     Completed   0          26h
installer-7-master-3.ocp.tektutor.org         0/1     Completed   0          26h
installer-8-master-1.ocp.tektutor.org         0/1     Completed   0          26h
installer-8-master-2.ocp.tektutor.org         0/1     Completed   0          26h
installer-8-master-3.ocp.tektutor.org         0/1     Completed   0          26h
revision-pruner-7-master-1.ocp.tektutor.org   0/1     Completed   0          26h
revision-pruner-7-master-2.ocp.tektutor.org   0/1     Completed   0          26h
revision-pruner-7-master-3.ocp.tektutor.org   0/1     Completed   0          26h
revision-pruner-8-master-1.ocp.tektutor.org   0/1     Completed   0          26h
revision-pruner-8-master-2.ocp.tektutor.org   0/1     Completed   0          26h
revision-pruner-8-master-3.ocp.tektutor.org   0/1     Completed   0          26h
(jegan@tektutor.org)$ <b>oc get pods -n openshift-kube-controller-manager</b>
NAME                                                      READY   STATUS      RESTARTS      AGE
installer-7-master-1.ocp.tektutor.org                     0/1     Completed   0             26h
installer-7-master-2.ocp.tektutor.org                     0/1     Completed   0             26h
installer-7-master-3.ocp.tektutor.org                     0/1     Completed   0             26h
installer-8-master-1.ocp.tektutor.org                     0/1     Completed   0             26h
installer-8-master-2.ocp.tektutor.org                     0/1     Completed   0             26h
installer-9-master-1.ocp.tektutor.org                     0/1     Completed   0             26h
installer-9-master-2.ocp.tektutor.org                     0/1     Completed   0             26h
installer-9-master-3.ocp.tektutor.org                     0/1     Completed   0             26h
kube-controller-manager-guard-master-1.ocp.tektutor.org   1/1     Running     0             26h
kube-controller-manager-guard-master-2.ocp.tektutor.org   1/1     Running     0             26h
kube-controller-manager-guard-master-3.ocp.tektutor.org   1/1     Running     0             26h
<b>kube-controller-manager-master-1.ocp.tektutor.org         4/4     Running     0             26h
kube-controller-manager-master-2.ocp.tektutor.org         4/4     Running     0             26h
kube-controller-manager-master-3.ocp.tektutor.org         4/4     Running     1 (26h ago)   26h</b>
revision-pruner-6-master-1.ocp.tektutor.org               0/1     Completed   0             26h
revision-pruner-6-master-2.ocp.tektutor.org               0/1     Completed   0             26h
revision-pruner-6-master-3.ocp.tektutor.org               0/1     Completed   0             26h
revision-pruner-7-master-1.ocp.tektutor.org               0/1     Completed   0             26h
revision-pruner-7-master-2.ocp.tektutor.org               0/1     Completed   0             26h
revision-pruner-7-master-3.ocp.tektutor.org               0/1     Completed   0             26h
revision-pruner-8-master-1.ocp.tektutor.org               0/1     Completed   0             26h
revision-pruner-8-master-2.ocp.tektutor.org               0/1     Completed   0             26h
revision-pruner-8-master-3.ocp.tektutor.org               0/1     Completed   0             26h
revision-pruner-9-master-1.ocp.tektutor.org               0/1     Completed   0             26h
revision-pruner-9-master-2.ocp.tektutor.org               0/1     Completed   0             26h
revision-pruner-9-master-3.ocp.tektutor.org               0/1     Completed   0             26h
(jegan@tektutor.org)$ <b>oc get pods -n openshift-kube-scheduler</b>
NAME                                                       READY   STATUS      RESTARTS   AGE
installer-7-master-1.ocp.tektutor.org                      0/1     Completed   0          26h
installer-7-master-2.ocp.tektutor.org                      0/1     Completed   0          26h
installer-8-master-1.ocp.tektutor.org                      0/1     Completed   0          26h
installer-8-master-2.ocp.tektutor.org                      0/1     Completed   0          26h
installer-8-master-3.ocp.tektutor.org                      0/1     Completed   0          26h
openshift-kube-scheduler-guard-master-1.ocp.tektutor.org   1/1     Running     0          26h
openshift-kube-scheduler-guard-master-2.ocp.tektutor.org   1/1     Running     0          26h
openshift-kube-scheduler-guard-master-3.ocp.tektutor.org   1/1     Running     0          26h
<b>openshift-kube-scheduler-master-1.ocp.tektutor.org         3/3     Running     0          26h
openshift-kube-scheduler-master-2.ocp.tektutor.org         3/3     Running     0          26h
openshift-kube-scheduler-master-3.ocp.tektutor.org         3/3     Running     0          26h</b>
revision-pruner-6-master-1.ocp.tektutor.org                0/1     Completed   0          26h
revision-pruner-6-master-2.ocp.tektutor.org                0/1     Completed   0          26h
revision-pruner-6-master-3.ocp.tektutor.org                0/1     Completed   0          26h
revision-pruner-7-master-1.ocp.tektutor.org                0/1     Completed   0          26h
revision-pruner-7-master-2.ocp.tektutor.org                0/1     Completed   0          26h
revision-pruner-7-master-3.ocp.tektutor.org                0/1     Completed   0          26h
revision-pruner-8-master-1.ocp.tektutor.org                0/1     Completed   0          26h
revision-pruner-8-master-2.ocp.tektutor.org                0/1     Completed   0          26h
revision-pruner-8-master-3.ocp.tektutor.org                0/1     Completed   0          26h
</pre>

## ⛹️‍♂️ Lab - Finding the nodes where a pod runs
```
oc get pods -n openshift-kube-scheduler -o wide
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>oc get pods -n openshift-kube-scheduler -o wide</b>
NAME                                                       READY   STATUS      RESTARTS   AGE   IP                NODE                        NOMINATED NODE   READINESS GATES
installer-7-master-1.ocp.tektutor.org                      0/1     Completed   0          26h   10.129.0.54       master-1.ocp.tektutor.org   <none>           <none>
installer-7-master-2.ocp.tektutor.org                      0/1     Completed   0          26h   10.128.0.20       master-2.ocp.tektutor.org   <none>           <none>
installer-8-master-1.ocp.tektutor.org                      0/1     Completed   0          26h   10.129.0.76       master-1.ocp.tektutor.org   <none>           <none>
installer-8-master-2.ocp.tektutor.org                      0/1     Completed   0          26h   10.128.0.43       master-2.ocp.tektutor.org   <none>           <none>
installer-8-master-3.ocp.tektutor.org                      0/1     Completed   0          26h   10.130.0.39       master-3.ocp.tektutor.org   <none>           <none>
openshift-kube-scheduler-guard-master-1.ocp.tektutor.org   1/1     Running     0          26h   10.129.0.56       master-1.ocp.tektutor.org   <none>           <none>
openshift-kube-scheduler-guard-master-2.ocp.tektutor.org   1/1     Running     0          26h   10.128.0.15       master-2.ocp.tektutor.org   <none>           <none>
openshift-kube-scheduler-guard-master-3.ocp.tektutor.org   1/1     Running     0          26h   10.130.0.44       master-3.ocp.tektutor.org   <none>           <none>
openshift-kube-scheduler-master-1.ocp.tektutor.org         3/3     Running     0          26h   192.168.122.39    master-1.ocp.tektutor.org   <none>           <none>
openshift-kube-scheduler-master-2.ocp.tektutor.org         3/3     Running     0          26h   192.168.122.184   master-2.ocp.tektutor.org   <none>           <none>
openshift-kube-scheduler-master-3.ocp.tektutor.org         3/3     Running     0          26h   192.168.122.74    master-3.ocp.tektutor.org   <none>           <none>
revision-pruner-6-master-1.ocp.tektutor.org                0/1     Completed   0          26h   10.129.0.39       master-1.ocp.tektutor.org   <none>           <none>
revision-pruner-6-master-2.ocp.tektutor.org                0/1     Completed   0          26h   10.128.0.13       master-2.ocp.tektutor.org   <none>           <none>
revision-pruner-6-master-3.ocp.tektutor.org                0/1     Completed   0          26h   10.130.0.12       master-3.ocp.tektutor.org   <none>           <none>
revision-pruner-7-master-1.ocp.tektutor.org                0/1     Completed   0          26h   10.129.0.41       master-1.ocp.tektutor.org   <none>           <none>
revision-pruner-7-master-2.ocp.tektutor.org                0/1     Completed   0          26h   10.128.0.19       master-2.ocp.tektutor.org   <none>           <none>
revision-pruner-7-master-3.ocp.tektutor.org                0/1     Completed   0          26h   10.130.0.16       master-3.ocp.tektutor.org   <none>           <none>
revision-pruner-8-master-1.ocp.tektutor.org                0/1     Completed   0          26h   10.129.0.62       master-1.ocp.tektutor.org   <none>           <none>
revision-pruner-8-master-2.ocp.tektutor.org                0/1     Completed   0          26h   10.128.0.37       master-2.ocp.tektutor.org   <none>           <none>
revision-pruner-8-master-3.ocp.tektutor.org                0/1     Completed   0          26h   10.130.0.40       master-3.ocp.tektutor.org   <none>           <none>
</pre>

## ⛹️‍♂️ Lab - Finding OS and Container Runtime installed in each of the OpenShift nodes
```
oc get nodes -o wide
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>oc get nodes -o wide</b>
NAME                        STATUS   ROLES           AGE   VERSION            INTERNAL-IP       EXTERNAL-IP   OS-IMAGE                                                        KERNEL-VERSION                 CONTAINER-RUNTIME
master-1.ocp.tektutor.org   Ready    master,worker   26h   v1.23.12+6b34f32   192.168.122.39    <none>        Red Hat Enterprise Linux CoreOS 410.84.202210130022-0 (Ootpa)   4.18.0-305.62.1.el8_4.x86_64   cri-o://1.23.3-17.rhaos4.10.git016b1ca.el8
master-2.ocp.tektutor.org   Ready    master,worker   26h   v1.23.12+6b34f32   192.168.122.184   <none>        Red Hat Enterprise Linux CoreOS 410.84.202210130022-0 (Ootpa)   4.18.0-305.62.1.el8_4.x86_64   cri-o://1.23.3-17.rhaos4.10.git016b1ca.el8
master-3.ocp.tektutor.org   Ready    master,worker   26h   v1.23.12+6b34f32   192.168.122.74    <none>        Red Hat Enterprise Linux CoreOS 410.84.202210130022-0 (Ootpa)   4.18.0-305.62.1.el8_4.x86_64   cri-o://1.23.3-17.rhaos4.10.git016b1ca.el8
worker-1.ocp.tektutor.org   Ready    worker          26h   v1.23.12+6b34f32   192.168.122.174   <none>        Red Hat Enterprise Linux CoreOS 410.84.202210130022-0 (Ootpa)   4.18.0-305.62.1.el8_4.x86_64   cri-o://1.23.3-17.rhaos4.10.git016b1ca.el8
worker-2.ocp.tektutor.org   Ready    worker          26h   v1.23.12+6b34f32   192.168.122.109   <none>        Red Hat Enterprise Linux CoreOS 410.84.202210130022-0 (Ootpa)   4.18.0-305.62.1.el8_4.x86_64   cri-o://1.23.3-17.rhaos4.10.git016b1ca.el8
</pre>


## Demo - Finding node level details in Kubernetes
```
kubectl get nodes -o wide
```

<pre>
jegan@worker1.tektutor.org:~/Desktop$ <b>kubectl get nodes -o wide</b>
NAME       STATUS   ROLES           AGE   VERSION   INTERNAL-IP    EXTERNAL-IP   OS-IMAGE             KERNEL-VERSION      CONTAINER-RUNTIME
minikube   Ready    control-plane   34m   v1.25.3   192.168.49.2   <none>        Ubuntu 20.04.5 LTS   5.15.0-52-generic   docker://20.10.20
</pre>

## ⛹️‍♂️ Lab - Creating a project in OpenShift
```
oc new-project jegan
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>oc new-project jegan</b>
Now using project "jegan" on server "https://api.ocp.tektutor.org:6443".

You can add applications to this project with the 'new-app' command. For example, try:

    oc new-app rails-postgresql-example

to build a new example application in Ruby. Or use kubectl to deploy a simple Kubernetes application:

    kubectl create deployment hello-node --image=k8s.gcr.io/e2e-test-images/agnhost:2.33 -- /agnhost serve-hostname
</pre>

## Lab - Finding the currently active project
```
oc project
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>oc project</b>
Using project "jegan" on server "https://api.ocp.tektutor.org:6443".
</pre>

## ⛹️‍♂️ Lab - Switching to a project
```
oc project default
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>oc project default</b>
Now using project "default" on server "https://api.ocp.tektutor.org:6443".
</pre>

## Lab - Deleting a project in OpenShift
Deleting a project will delete everything that is created within the project.
```
oc delete project jegan
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>oc delete project jegan</b>
project.project.openshift.io "jegan" deleted
</pre>


## Lab - Deploying your first application within RedHat OpenShift cluster
```
oc project jegan
oc create deployment nginx --image=bitnami/nginx:latest --replicas=3
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>oc create deployment nginx --image=nginx:latest --replicas=3 </b>
deployment.apps/nginx created
</pre>

Listing the deployments
```
oc get deployments
oc get deployment
oc get deploy
kubectl get deployments
kubectl get deployment
kubectl get deploy
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>oc get deployments</b>
NAME    READY   UP-TO-DATE   AVAILABLE   AGE
nginx   0/3     3            0           13s
(jegan@tektutor.org)$ <b>oc get deployment</b>
NAME    READY   UP-TO-DATE   AVAILABLE   AGE
nginx   0/3     3            0           19s
(jegan@tektutor.org)$ <b>oc get deploy</b>
NAME    READY   UP-TO-DATE   AVAILABLE   AGE
nginx   0/3     3            0           23s
(jegan@tektutor.org)$ <b>kubectl get deployments</b>
NAME    READY   UP-TO-DATE   AVAILABLE   AGE
nginx   0/3     3            0           35s
(jegan@tektutor.org)$ <b>kubectl get deployment</b>
NAME    READY   UP-TO-DATE   AVAILABLE   AGE
nginx   0/3     3            0           39s
(jegan@tektutor.org)$ <b>kubectl get deploy</b>
NAME    READY   UP-TO-DATE   AVAILABLE   AGE
nginx   0/3     3            0           43s
</pre>

Listing the pods
```
oc get pods -w
oc get pod
oc get po
kubectl get pods -w
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>kubectl get pods -w</b>
NAME                     READY   STATUS   RESTARTS      AGE
nginx-7c658794b9-6q96h   0/1     Error    2 (24s ago)   63s
nginx-7c658794b9-7cnjh   0/1     Error    2 (26s ago)   63s
nginx-7c658794b9-w5pj5   0/1     Error    2 (24s ago)   63s
nginx-7c658794b9-w5pj5   0/1     CrashLoopBackOff   2 (12s ago)   70s
nginx-7c658794b9-7cnjh   0/1     CrashLoopBackOff   2 (13s ago)   71s
nginx-7c658794b9-6q96h   0/1     CrashLoopBackOff   2 (16s ago)   75s
nginx-7c658794b9-w5pj5   0/1     Error              3 (32s ago)   90s
nginx-7c658794b9-6q96h   0/1     Error              3 (32s ago)   91s
nginx-7c658794b9-7cnjh   0/1     Error              3 (33s ago)   91s
</pre>

Checking the logs to troubleshoot the issue
```
oc logs nginx-7c658794b9-7cnjh
```

<pre>
(jegan@tektutor.org)$ <b>oc logs nginx-7c658794b9-7cnjh</b>
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: info: can not modify /etc/nginx/conf.d/default.conf (read-only file system?)
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
2022/11/08 05:45:14 [warn] 1#1: the "user" directive makes sense only if the master process runs with super-user privileges, ignored in /etc/nginx/nginx.conf:2
nginx: [warn] the "user" directive makes sense only if the master process runs with super-user privileges, ignored in /etc/nginx/nginx.conf:2
2022/11/08 05:45:14 [emerg] 1#1: <b>mkdir() "/var/cache/nginx/client_temp" failed (13: Permission denied)</b>
nginx: [emerg] mkdir() "/var/cache/nginx/client_temp" failed (13: Permission denied)
(jegan@tektutor.org)$ <b>kubectl get pods -w</b>
NAME                     READY   STATUS             RESTARTS        AGE
nginx-7c658794b9-6q96h   0/1     CrashLoopBackOff   10 (116s ago)   29m
nginx-7c658794b9-7cnjh   0/1     CrashLoopBackOff   10 (119s ago)   29m
nginx-7c658794b9-w5pj5   0/1     CrashLoopBackOff   10 (108s ago)   29m
</pre>

## ⛹️‍♂️ Lab - Let's delete the deployment and redeploy nginx using bitnami/nginx docker image

Let's delete the existing nginx deployment
```
oc delete deploy nginx
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>oc delete deploy nginx</b>
deployment.apps "nginx" deleted
</pre>

Let's deploy nginx using bitnami/nginx docker image that follows the OpenShift Container Image best practices
```
oc create deployment nginx --image=bitnami/nginx:latest --replicas=3
```
Expected output
<pre>
(jegan@tektutor.org)$ <b>oc create deployment nginx --image=bitnami/nginx:latest --replicas=3</b>
deployment.apps/nginx created
</pre>


Let's list the deployments
```
oc get deploy
```
Expected output
<pre>
(jegan@tektutor.org)$ <b>oc get deploy</b>
NAME    READY   UP-TO-DATE   AVAILABLE   AGE
nginx   0/3     3            0           3s
</pre>

Let's list the replicasets
```
oc get replicasets
oc get replicaset
oc get rs
```
Expected output
<pre>
(jegan@tektutor.org)$ <b>oc get replicasets</b>
NAME               DESIRED   CURRENT   READY   AGE
nginx-78644964b4   3         3         3       8s
(jegan@tektutor.org)$ <b>oc get replicaset</b>
NAME               DESIRED   CURRENT   READY   AGE
nginx-78644964b4   3         3         3       13s
(jegan@tektutor.org)$ <b>oc get rs</b>
NAME               DESIRED   CURRENT   READY   AGE
nginx-78644964b4   3         3         3       16s
</pre>

Let's list the pods
```
oc get po -w
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>oc get po</b>
NAME                     READY   STATUS    RESTARTS   AGE
nginx-78644964b4-9sms6   1/1     Running   0          22s
nginx-78644964b4-bv7n5   1/1     Running   0          22s
nginx-78644964b4-hqtdx   1/1     Running   0          22s
</pre>

Checking nginx pod logs
```
oc logs nginx-78644964b4-hqtdx
```
Expected output
<pre>
(jegan@tektutor.org)$ <b>oc logs nginx-78644964b4-hqtdx</b>
nginx 06:36:37.44 
nginx 06:36:37.44 Welcome to the Bitnami nginx container
nginx 06:36:37.45 Subscribe to project updates by watching https://github.com/bitnami/containers
nginx 06:36:37.45 Submit issues and feature requests at https://github.com/bitnami/containers/issues
nginx 06:36:37.45 
nginx 06:36:37.46 INFO  ==> ** Starting NGINX setup **
nginx 06:36:37.48 INFO  ==> Validating settings in NGINX_* env vars
nginx 06:36:37.50 INFO  ==> No custom scripts in /docker-entrypoint-initdb.d
nginx 06:36:37.51 INFO  ==> Initializing NGINX
realpath: /bitnami/nginx/conf/vhosts: No such file or directory
nginx 06:36:37.57 INFO  ==> ** NGINX setup finished! **

nginx 06:36:37.59 INFO  ==> ** Starting NGINX **
</pre>

## Points to rembers - What happens within OpenShift when we create a deployment?
```
oc create deployment nginx --image=bitnami/nginx:latest --replicas=3
```

<pre>
1. oc client tool make REST API call to API server, requesting to create a deployment with name 'nginx' using the container image 'bitnami/nginx:latest'
2. API Server receives the REST call from oc client tool, it then creates a Deployment record within etcd data-store.
3. API Server then triggers an event something like "New Deployment Created"
4. Deployment Controller receives the "New Deployment Created" event, it then makes a REST call to API Server requesting it to create a ReplicaSet with a name nginx-<random-unique-pod-hash-template>.
5. API Server receives the REST call request from Deployment Controller, it then creates a ReplicaSet record in the etcd data-store.
6. API Server then triggers an event something like "New ReplicaSet Created"
7. ReplicaSet Controller receives the event, it then makes REST call to API Server, requesting to create 3 Pods.
8. API Server receives the request from ReplicaSet Controller, it then creates 3 Pod records(definitions) in the etcd database.
9. API Server then triggers an event something like "New Pod Created".
10. Scheduler receives this event, it then identifies healthy nodes where those 3 Pods can be deployed.
11. Scheduler sends the scheduling recommendations of the 3 Pods as 3 different REST call to API Server.
12. API Server receives the request from Scheduler, API Server retrieves the Pod entry from etcd database, updates the scheduling recommendations that was recommended by Scheduler.
13. ApI Server then triggers an avent something like "Pod Scheduled".
14. Kubelet Container Agent that runs on every node receives this event, if it detects the pod is scheduled on the node where the kubelet is running, it then pulls the respective Container Image.  Kubelet interacts the CRI-O Container Runtime and creates the Containers that are associated with the Pod.
15. Kubelet monitors the status and health of the Containers running on the current node, it keeps reporting the status as to the API Server frequently(heart-beat) via REST API calls.
16. API Server receives the REST calls status updates from kubelet, it retrieves the Pod entry from the etcd data-store and updates the status.
</pre>

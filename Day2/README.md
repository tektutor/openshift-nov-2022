# Day 2 ( 4 Hours )

## Lab - Listing OpenShift nodes in the cluster
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


## Lab - Listing the projects
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

## Lab - Finding more details about your master node
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

## Lab - Listing Control Plane components in OpenShift
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

## Lab - Finding the nodes where a pod runs
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

## Lab - Finding OS and Container Runtime installed in each of the OpenShift nodes
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

## Lab - Creating a project in OpenShift
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

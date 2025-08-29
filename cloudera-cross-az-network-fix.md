# Fix: Cloudera on Cloud — Cross-AZ Connectivity Failure (Calico IPPool)
This playbook documents a quick fix when Pods cannot communicate across Availability Zones (AZs) because the Calico IPPool is using IP-in-IP instead of VXLAN. <br>

### Symptoms <br>
	•	Pods scheduled on different AZs fail to reach each other. <br>
	•	Some Pods are stuck Pending / CrashLoopBackOff / NotReady. <br>
	•	Service-to-pod traffic works only within the same AZ. <br>

### Solution <br>
List all non-Running pods (across all namespaces): <br>
```bash
kubectl get pod -A -o wide
kubectl get nodes -L topology.kubernetes.io/zone
```
Edit IPPOOL <br>
```bash
kubectl edit ippool
```

change from the original `ipipMode: CrossSubnet and vxlanMode: Never` <br>
to `ipipMode: Never and vxlanMode: CrossSubnet`
```bash
  spec:
    ipipMode: Never
    vxlanMode: CrossSubnet
```

### Verify
```bash
kubectl get pod -n logging
```

---
title: Verify ingress-controller Chart Installation
description: This tutorial explains how to verify that ingress-controller chart installed successfully
---


Once the helm chart installation done you need to verify all the pods and services are up and running.

Execute below command to check status of pods and services: 

### Check the pod status


```execute
kubectl get pods --namespace ingress-controller
```

You will see similar to this output:

```

```

It may take few minutes to change the `STATUS` from `ContainerCreating` to `Running`. 

```output

```

Once the `ingress-controller` PODs are up and running , and `READY` states are `1/1`, your ingress-controller is ready to use.

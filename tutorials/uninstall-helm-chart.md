---
title: How to Uninstall ingress-controller Helm chart 
description: This tutorial explains how to Uninstall ingress-controller helm chart
---

### Uninstall ingress-controller Helm Chart

Check your deployed ingress-controller helm chart:

```execute
 helm list -n ingress-controller
```

 Output will be similar:

```output

```

To uninstall the ingress-controller helm chart, use the helm delete command:

```execute
helm delete ingress-controller -n ingress-controller
```

Output:

```output
release "ingress-controller" uninstalled
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

```execute
helm status ingress-controller -n ingress-controller
```

Output:

```
Error: release: not found
```

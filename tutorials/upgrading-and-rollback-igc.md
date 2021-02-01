---
title: How to Upgrade and Rollback ingress-controller chart 
description: This tutorial explains how to Upgrade and Rollback ingress-controller helm chart
---



### Upgrading ingress-controller

It is also important to keep ingress-controller helm chart updated. 

To Upgrade ingress-controller helm chart,please execute following steps:

**Step 1:** To list all of your current releases in namespace "ingress-controller", run the following command :

```execute
helm list -n ingress-controller
```
You should get output similar to this:

Output:
```
 
```

As you can see from the output, our current ingress-controller (ingress-controller) version is 2.7.0 (app version), while the chart version is 12.5.0 which is the latest one.
But suppose your current helm chart version is 12.4.0 and app version is 2.6.0 and is not updated one and you need to upgrade it. 

You need to follow below steps.

**Step 2:** Update your Helm repositories with following command :

```execute
helm repo update 
```

You can expect the following output:

```output

```

**Step 3:** Check if there’s a newer version of the Ingress-controller chart available using following command:

```execute
helm search repo ingress-controller --versions
```

You should see output similar to this:

```

```

As you can see from the output, there’s a new chart available (version 12.5.0) with ingress-controller 2.7.0 (app version). 

**Step 4:** To upgrade your ingress-controller release to the latest ingress-controller chart, execute below command:

```execute
helm upgrade ingress-controller bitnami/nginx-ingress-controller --namespace ingress-controller
```

This command will produce output very similar to the output produced by helm install:

```output
```

**Step 5:** Check updated information about your release using helm list command.

```execute
helm list -n ingress-controller
```

Output:
```

```

You have successfully upgraded your ingress-controller to the latest version of the ingress-controller chart.


### Rolling Back a Release

Each time you upgrade a release, a new revision of that release is created by Helm. A revision sets a fixed checkpoint to where you can come back if things don’t work as expected. 
If something goes wrong during the upgrade process, you can always rollback to a previous revision of a given Helm release with the helm rollback command.


If we want to undo the upgrade and rollback our ingress-controller release to its previous version, execute following command:

Execute rollback command :

```execute
helm rollback ingress-controller 1 -n ingress-controller
```

Output:

```output
Rollback was a success! Happy Helming!
```

This would rollback the ingress-controller installation to its previous release. 

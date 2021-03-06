**What is Kubernetes ingress?**

Kubernetes ingress is a collection of routing rules that govern how external users access services running in a Kubernetes cluster. However, in real-world Kubernetes deployments, there are frequently additional considerations beyond routing for managing ingress. We’ll discuss these requirements in more detail below.


**Ingress in Kubernetes**

In Kubernetes, there are three general approaches to exposing your application.

1-Using a Kubernetes service of type NodePort, which exposes the application on a port across each of your nodes

2-Use a Kubernetes service of type LoadBalancer, which creates an external load balancer that points to a Kubernetes service in your cluster

3-Use a Kubernetes Ingress Resource


**NodePort**

A NodePort is an open port on every node of your cluster. Kubernetes transparently routes incoming traffic on the NodePort to your service, even if your application is running on a different node.


Every Kubernetes cluster supports NodePort, although if you’re running in a cloud provider such as Google Cloud, you may have to edit your firewall rules. However, a NodePort is assigned from a pool of cluster-configured NodePort ranges (typically 30000–32767). While this is likely not a problem for most TCP or UDP clients, HTTP or HTTPS traffic end up being exposed on a non-standard port.


The NodePort abstraction is intended to be a building block for higher-level ingress models (e.g., load balancers). It is handy for development purposes, however, when you don’t need a production URL.

**Load Balancer**

Using a LoadBalancer service type automatically deploys an external load balancer. This external load balancer is associated with a specific IP address and routes external traffic to a Kubernetes service in your cluster.


The exact implementation of a LoadBalancer is dependent on your cloud provider, and not all cloud providers support the LoadBalancer service type. Moreover, if you’re deploying Kubernetes on bare metal, you’ll have to supply your own load balancer implementation. That said, if you’re in an environment that supports the LoadBalancer service type, this is likely the safest, simplest way to route your traffic.Ingress Controllers and Ingress Resources

**Ingress Controllers and Ingress Resources**

Kubernetes supports a high level abstraction called Ingress, which allows simple host or URL based HTTP routing. An ingress is a core concept (in beta) of Kubernetes, but is always implemented by a third party proxy. These implementations are known as ingress controllers. An ingress controller is responsible for reading the Ingress Resource information and processing that data accordingly. Different ingress controllers have extended the specification in different ways to support additional use cases.


Ingress is tightly integrated into Kubernetes, meaning that your existing workflows around kubectl will likely extend nicely to managing ingress. Note that an ingress controller typically doesn’t eliminate the need for an external load balancer — the ingress controller simply adds an additional layer of routing and control behind the load balancer.

**Service-specific ingress management**

So the question for your ingress strategy is really about choosing the right way to manage traffic from your external load balancer to your services. What are your options?


You can choose an ingress controller such as ingress-nginx or NGINX kubernetes-ingress

You can choose an API Gateway deployed as a Kubernetes service such as Ambassador (built on Envoy) or Traefik.

You can deploy your own using a custom configuration of NGINX, HAProxy, or Envoy.


**Lets Enable the Ingress controller**

1-To enable the NGINX Ingress controller, run the following command:

```
minikube addons enable ingress
```

2-Verify that the NGINX Ingress controller is running

```
kubectl get pods -n kube-system
```
**Note:** This can take up to a minute.

Output:

```
NAME                                        READY     STATUS    RESTARTS   AGE
default-http-backend-59868b7dd6-xb8tq       1/1       Running   0          1m
kube-addon-manager-minikube                 1/1       Running   0          3m
kube-dns-6dcb57bcc8-n4xd4                   3/3       Running   0          2m
kubernetes-dashboard-5498ccf677-b8p5h       1/1       Running   0          2m
nginx-ingress-controller-5984b97644-rnkrg   1/1       Running   0          1m
storage-provisioner                         1/1       Running   0          2m

```

**Lets Deploy a hello, world app**

1-Create a Deployment using the following command:

```
kubectl create deployment web --image=gcr.io/google-samples/hello-app:1.0
```
Output:

```
deployment.apps/web created
```

2-Expose the Deployment:

```
kubectl expose deployment web --type=NodePort --port=8080
```
Output:

```
service/web exposed
```

3-Verify the Service is created and is available on a node port:
```
kubectl get service web
```
Output:

```
NAME      TYPE       CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE
web       NodePort   169.62.52.103   <none>        8080:31637/TCP   12m

```
4-Visit the service via NodePort:

```
minikube service web --url
```

###NGINX Ingress Controller

This repo provides an implementation of an Ingress controller for NGINX and NGINX Plus.

**Note:** this project is different from the NGINX Ingress controller in kubernetes/ingress-nginx repo. See this doc to find out about the key differences.

**What is the Ingress?**
The Ingress is a Kubernetes resource that lets you configure an HTTP load balancer for applications running on Kubernetes, represented by one or more Services. Such a load balancer is necessary to deliver those applications to clients outside of the Kubernetes cluster.

**The Ingress resource supports the following features:**

**Content-based routing:**

Host-based routing. For example, routing requests with the host header foo.example.com to one group of services and the host header bar.example.com to another group.
Path-based routing. For example, routing requests with the URI that starts with /serviceA to service A and requests with the URI that starts with /serviceB to service B.
TLS/SSL termination for each hostname, such as foo.example.com

**NGINX Ingress Controller**

NGINX Ingress controller works with both NGINX and NGINX Plus and supports the standard Ingress features - content-based routing and TLS/SSL termination.

Additionally, several NGINX and NGINX Plus features are available as extensions to the Ingress resource via annotations and the ConfigMap resource. In addition to HTTP, NGINX Ingress controller supports load balancing Websocket, gRPC, TCP and UDP applications. See ConfigMap and Annotations docs to learn more about the supported features and customization options.

As an alternative to the Ingress, NGINX Ingress controller supports the VirtualServer and VirtualServerRoute resources. They enable use cases not supported with the Ingress resource, such as traffic splitting and advanced content-based routing. See VirtualServer and VirtualServerRoute resources doc.

TCP, UDP and TLS Passthrough load balancing is also supported. 


**NGINX Ingress Controller Releases**

The latest stable release is 1.10.0. For production use, we recommend that you choose the latest stable release. As an alternative, you can choose the edge version built from the latest commit from the master branch. The edge version is useful for experimenting with new features that are not yet published in a stable release.
git clone https://github.com/nginxinc/kubernetes-ingress/
To use the Ingress controller, you need to have access to:

An Ingress controller image.

Installation manifests or a Helm chart.

**Documentation and examples.**

It is important that the versions of those things above match.

The table below summarizes the options regarding the images, manifests, helm chart, documentation and examples and gives your links to the correct versions:


**Installation with Helm:**

Getting the Chart Sources

This step is required if you’re installing the chart using its sources. Additionally, the step is also required for managing the custom resource definitions (CRDs), which the Ingress Controller requires by default, or for upgrading/deleting the CRDs.


1-Clone the Ingress controller repo:

```
git clone https://github.com/nginxinc/kubernetes-ingress/
```


2-Change your working directory to /deployments/helm-chart:

```
cd kubernetes-ingress/deployments/helm-chart
```

```
git checkout v1.10.0
```

**Adding the Helm Repository**

This step is required if you’re installing the chart via the helm repository.

```
helm repo add nginx-stable https://helm.nginx.com/stable
```

```
helm repo update
```

**Create the NginxIngressController manifest**

Create a manifest nginx-ingress-controller.yaml with the following content:

```
apiVersion: k8s.nginx.org/v1alpha1
kind: NginxIngressController
metadata:
name: my-nginx-ingress-controller
namespace: default
spec:
type: deployment
image:
repository: nginx/nginx-ingress
tag: 1.9.1
pullPolicy: Always
serviceType: NodePort
nginxPlus: False

```

**Note:** For NGINX Plus, change the image.repository and image.tag values and change nginxPlus to True.

**Create the NginxIngressController**

```
kubectl apply -f nginx-ingress-controller.yaml
```

A new instance of the NGINX Ingress Controller will be deployed by the NGINX Ingress Operator in the default namespace with default parameters

**NginxIngressController Custom Resource**

The NginxIngressController Custom Resource is the definition of a deployment of the Ingress Controller. With this Custom Resource, the NGINX Ingress Operator will be able to deploy and configure instances of the Ingress Controller in your cluster.

**Configuration**

There are several fields to configure the deployment of an Ingress Controller.

The following example shows the minimum configuration using only required fields:

```
apiVersion: k8s.nginx.org/v1alpha1
kind: NginxIngressController
metadata:
name: my-nginx-ingress-controller
namespace: my-nginx-ingress
spec:
type: deployment
image:
repository: nginx/nginx-ingress
tag: edge
pullPolicy: Always
serviceType: NodePort

```


The following example shows the usage of all fields (required and optional):

```
apiVersion: k8s.nginx.org/v1alpha1
 kind: NginxIngressController
 metadata:
   name: my-nginx-ingress-controller
   namespace: my-nginx-ingress
 spec:
   type: deployment
   nginxPlus: false
   image:
     repository: nginx/nginx-ingress
     tag: edge
     pullPolicy: Always
   replicas: 3
   serviceType: NodePort
   enableCRDs: true
   enableSnippets: false
   defaultSecret: my-nginx-ingress/default-secret
   ingressClass: my-nginx-ingress
   useIngressClassOnly: true
   watchNamespace: default
   healthStatus:
     enable: true
     uri: "/my-health"
   nginxDebug: true
   logLevel: 3
   nginxStatus:
     enable: true
     port: 9090
     allowCidrs: "127.0.0.1"
   enableLeaderElection: true
   wildcardTLS: my-nginx-ingress/wildcard-secret
   reportIngressStatus:
     enable: true
     externalService: my-nginx-ingress
   prometheus:
     enable: true
     port: 9114
   enableLatencyMetrics: false
   configMapData:
     error-log-level: debug
   enableTLSPassthrough: true
   globalConfiguration: my-nginx-ingress/nginx-configuration
   nginxReloadTimeout: 5000
   appProtect:
     enable: false
     
 ```


**Installing the Chart**

By default, the Ingress Controller requires a number of custom resource definitions (CRDs) installed in the cluster. The Helm client will install those CRDs.

If you do not use the custom resources that require those CRDs (which corresponds to controller.enableCustomResources set to false and controller.appprotect.enable set to false). The installation of the CRDs can be skipped by specifying --skip-crds for the helm install command.


**Installing via Helm Repository**

To install the chart with the release name my-release (my-release is the name that you choose):

**For NGINX:**

```
helm install my-release nginx-stable/nginx-ingress
```

For NGINX Plus: (assuming you have pushed the Ingress controller image nginx-plus-ingress to your private registry myregistry.example.com)

```
helm install my-release nginx-stable/nginx-ingress --set controller.image.repository=myregistry.example.com/nginx-plus-ingress --set controller.nginxplus=true
```

**Installing Using Chart Sources**

```
helm install my-releas
```

For NGINX Plus:

```
helm install my-release -f values-plus.yaml .
```

The command deploys the Ingress controller in your Kubernetes cluster in the default configuration. The configuration section lists the parameters that can be configured during installation.

When deploying the Ingress controller, make sure to use your own TLS certificate and key for the default server rather than the default pre-generated ones. Read the Configuration section below to see how to configure a TLS certificate and key for the default server. Note that the default server returns the Not Found page with the 404 status code for all requests for domains for which there are no Ingress rules defined.


***Upgrading the Chart**

**Note:** The following warning is expected and can be ignored: Warning: kubectl apply should be used on resource created by either kubectl create --save-config or kubectl apply.

**Upgrading the Release**

To upgrade the release my-release:

**Upgrade Using Chart Sources:**

```
helm upgrade my-release .
```

**Upgrade via Helm Repository:**

```
helm upgrade my-release nginx-stable/nginx-ingress
```

###Uninstalling the Chart

**Uninstalling the Release**

To uninstall/delete the release my-release:

```
helm uninstall my-release
```

The command removes all the Kubernetes components associated with the release.

**Uninstalling the CRDs**

Uninstalling the release does not remove the CRDs. To remove the CRDs, run:

```
kubectl delete -f crds/
```

**Note:** This command will delete all the corresponding custom resources in your cluster across all namespaces. Please ensure there are no custom resources that you want to keep and there are no other Ingress Controller releases running in the cluster.


**Running Multiple Ingress Controllers**

If you are running multiple Ingress Controller releases in your cluster with enabled custom resources, the releases will share a single version of the CRDs. As a result, make sure that the Ingress Controller versions match the version of the CRDs. Additionally, when uninstalling a release, ensure that you don’t remove the CRDs until there are no other Ingress Controller releases running in the cluster.

When running NGINX Ingress Controller, you have the following options with regards to which configuration resources it handles:

**Cluster-wide Ingress Controller (default).** The Ingress Controller handles configuration resources created in any namespace of the cluster. As NGINX is a high-performance load balancer capable of serving many applications at the same time, this option is used by default in our installation manifests and Helm chart.


**Single-namespace Ingress Controller.** You can configure the Ingress Controller to handle configuration resources only from a particular namespace, which is controlled through the -watch-namespace command-line argument. This can be useful if you want to use different NGINX Ingress Controllers for different applications, both in terms of isolation and/or operation.


**Ingress Controller for Specific Ingress Class.** This option works in conjunction with either of the options above. You can further customize which configuration resources are handled by the Ingress Controller by configuring the class of the Ingress Controller and using that class in your configuration resources.


Considering the options above, you can run multiple NGINX Ingress Controllers, each handling a different set of configuration resources.


**Running NGINX Ingress Controller and Another Ingress Controller**

It is possible to run NGINX Ingress Controller and an Ingress Controller for another load balancer in the same cluster. This is often the case if you create your cluster through a cloud provider managed Kubernetes service that by default might include the Ingress Controller for the HTTP load balancer of the cloud provider, and you want to use NGINX Ingress Controller.

To make sure that NGINX Ingress Controller handles particular configuration resources, update those resources with the class set to nginx or the value that you configured.


```
 helm install ingress-controller \
    --set image.pullPolicy=Always \
    bitnami/nginx-ingress-controller -n ingress-controller
```

The above command sets the `image.pullPolicy` to `Always`.

Alternatively, a YAML file that specifies the values for the parameters can be provided while installing the chart. For example,

```bash
$ helm install ingress-controller -f values.yaml bitnami/nginx-ingress-controller -n ingress-controller
```

> **Tip**: You can use the default [values.yaml](values.yaml)

## Configuration and installation details

### [Rolling VS Immutable tags](https://docs.bitnami.com/containers/how-to/understand-rolling-tags-containers/)

It is strongly recommended to use immutable tags in a production environment. This ensures your deployment does not change automatically if the same tag is updated with a different image.

Bitnami will release a new chart updating its containers if a new version of the main container, significant changes, or critical vulnerabilities exist.

### Sidecars and Init Containers

If you have a need for additional containers to run within the same pod as the NGINX Ingress Controller (e.g. an additional metrics or logging exporter), you can do so via the `sidecars` config parameter. Simply define your container according to the Kubernetes container spec.

```yaml

sidecars:
  - name: your-image-name
    image: your-image
    imagePullPolicy: Always
    ports:
      - name: portname
       containerPort: 1234
```

Similarly, you can add extra init containers using the `initContainers` parameter.

```yaml

initContainers:
  - name: your-image-name
    image: your-image
    imagePullPolicy: Always
    ports:
      - name: portname
        containerPort: 1234
```

### Deploying extra resources

There are cases where you may want to deploy extra objects, such a ConfigMap containing your app's configuration or some extra deployment with a micro service used by your app. For covering this case, the chart allows adding the full specification of other objects using the `extraDeploy` parameter.

### Setting Pod's affinity

This chart allows you to set your custom affinity using the `affinity` parameter. Find more information about Pod's affinity in the [kubernetes documentation](https://kubernetes.io/docs/concepts/configuration/assign-pod-node/#affinity-and-anti-affinity).

As an alternative, you can use of the preset configurations for pod affinity, pod anti-affinity, and node affinity available at the [bitnami/common](https://github.com/bitnami/charts/tree/master/bitnami/common#affinities) chart. To do so, set the `podAffinityPreset`, `podAntiAffinityPreset`, or `nodeAffinityPreset` parameters.

## Troubleshooting

Find more information about how to deal with common errors related to Bitnami’s Helm charts in [this troubleshooting guide](https://docs.bitnami.com/general/how-to/troubleshoot-helm-chart-issues).

## Notable changes

### 5.3.0

In this version you can indicate the key to download the GeoLite2 databases using the [parameter](#parameters) `maxmindLicenseKey`.



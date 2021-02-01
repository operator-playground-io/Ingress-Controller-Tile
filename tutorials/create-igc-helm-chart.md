# Nginx Ingress Controller

[nginx-ingress](https://github.com/kubernetes/ingress-nginx) is an Ingress controller that uses NGINX to manage external access to HTTP services in a Kubernetes cluster.

## TL;DR

```bash
$ helm repo add bitnami https://charts.bitnami.com/bitnami
$ helm install ingress-controller bitnami/nginx-ingress-controller -n ingress-controller
```

## Introduction

Bitnami charts for Helm are carefully engineered, actively maintained and are the quickest and easiest way to deploy containers on a Kubernetes cluster that are ready to handle production workloads.

This chart bootstraps a [nginx-ingress](https://github.com/kubernetes/ingress-nginx) deployment on a [Kubernetes](http://kubernetes.io) cluster using the [Helm](https://helm.sh) package manager.

Bitnami charts can be used with [Kubeapps](https://kubeapps.com/) for deployment and management of Helm Charts in clusters. This Helm chart has been tested on top of [Bitnami Kubernetes Production Runtime](https://kubeprod.io/) (BKPR). Deploy BKPR to get automated TLS certificates, logging and monitoring for your applications.

## Prerequisites

- Kubernetes 1.12+
- Helm 3.1.0

## Installing the Chart

To install the chart with the release name `ingress-controller`:

```bash
$ helm repo add bitnami https://charts.bitnami.com/bitnami
$ kubectl get ns ingress-controller
$ helm install ingress-controller bitnami/nginx-ingress-controller --set service.type=nodePort -n ingress-controller
```

These commands deploy nginx-ingress-controller on the Kubernetes cluster in the default configuration.

> **Tip**: List all releases using `helm list -n ingress-controller`
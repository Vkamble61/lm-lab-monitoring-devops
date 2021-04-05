# Observability with Monitoring and Logging 

This repository contains the instructions for getting [Prometheus monitoring](https://prometheus.io/) set up on your cluster and centralised logging configured using [Fluentd](https://www.fluentd.org/)

## Instructions

### Pre-requisites

* You'll need your Kubernetes cluster up and running with ArgoCD deployed to it.

* We'll be making edits to the devops-bookstore-api application so, if you haven't already, you'll need to make sure you have [forked the repository](https://github.com/techreturners/devops-bookstore-api) to your own GitHub account.

* You'll need to refresh yourself with building and deploying (via ArgoCD) your bookstore API to the cluster. For this you can utilise manual building of the docker image on your own machine **or** the CircleCI route as covered in session 5.

#### Supporting notes on pre-requisites

* Provisioning cluster: If you need a reminder on how to provision the cluster you can follow through the instructions on these links:
    * Amazon EKS [Instructions](https://github.com/techreturners/devops-upskill-eks-terraform/tree/session-004-gitops#readme)
    * Google GKE [Instructions](https://github.com/techreturners/devops-upskill-gke-terraform/tree/session-004-gitops#readme)
    * Azure AKS [Instructions](https://github.com/techreturners/devops-upskill-aks-terraform/tree/session-004-gitops#readme)

From this point onwards the instructions will assume the following:

* Your kubernetes cluster is up and running
* You have configured `kubectl` to be able to talk to your cluster
* Your cluster is running ArgoCD
* You have forked the devops-bookstore-api repository so that you can make changes to the code and push back to your own repository
* You are able to build a docker image of your bookstore API and push back to your container registry (either manually or via CircleCI)
* You know how to port-forward using `kubectl` so that you can access ArgoCD via your browser
* ArgoCD has been configured to be able to deploy your bookstore API

### Step 1 - Deploying the prometheus stack

The first step will require us to deploy the various Prometheus services, pods, volumes etc. As you might have guessed, we shall be continuing to build on existing knowledge and deploy this stack as a Helm chart using ArgoCD.

Thankfully the prometheus community already has a [ready made Helm chart](https://github.com/prometheus-community/helm-charts/tree/main/charts/kube-prometheus-stack) for us.

In this directory you'll see that we have taken a copy of the **kube-prometheus-stack** Helm chart which includes all the required setup for the prometheus stack. 

Open up ArgoCD in your browser - [this link should work](https://127.0.0.1:9000/applications) if you've configured the port-forwarding.

Click on **New App** shown in the top left of the screen.

Enter **kube-prometheus-stack** for the **application name**

Choose **default** for the **project**

You can leave the **Sync Policy** and **Sync Options** as defined.

Enter **https://github.com/techreturners/devops-upskill-monitoring-logging.git** for the **Repository URL**

Choose the **kube-prometheus-stack** option for the **stack**

Choose the **https://kubernetes.default.svc** for the **Cluster URL**

Enter **default** for the **Namespace**

The **Helm** section can all be left as default

Then click the **Create** button shown towards to top of the dialog you are viewing

Argo will take a moment before going back to the **Applications** screen where you will see a new application called **kube-prometheus-stack**

Now you can click the **Sync** button followed by **Synchronize** to deploy the full prometheus stack across your cluster.

After a few moments you should be able to see prometheus pods running on your cluster by running `kubectl get pods` which should output something similar to:

```
NAME                                                     READY   STATUS    RESTARTS   AGE
alertmanager-kube-prom-stack-kube-prome-alertmanager-0   2/2     Running   0          2m50s
kube-prom-stack-grafana-857f644995-nm5l4                 2/2     Running   0          3m2s
kube-prom-stack-kube-prome-operator-6479d4b85b-ftglr     1/1     Running   0          3m2s
kube-prom-stack-kube-state-metrics-cb9c4759f-t962s       1/1     Running   0          3m2s
kube-prom-stack-prometheus-node-exporter-59gjl           1/1     Running   0          3m3s
kube-prom-stack-prometheus-node-exporter-glf2q           1/1     Running   0          3m3s
kube-prom-stack-prometheus-node-exporter-t8mnz           1/1     Running   0          3m3s
prometheus-kube-prom-stack-kube-prome-prometheus-0       2/2     Running   1          2m44s
```








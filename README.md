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

First we'll get the Prometheus stack deployed to your cluster.

Head over to the [monitoring documentation](./docs/MONITORING.md) on steps for deploying this stack

### Step 2 - Instrumenting your application

Now you've explored the deployment of Prometheus, lets add some custom monitoring to the backend books API.

In this section imagine you've got your developer hat on and are working on code to make your applications more observable. In turn helping you learn about their performance and your operations teams monitoring of them.

Head over to the [instrumentation documentation](./docs/INSTRUMENTATION.md) for instructions and information on this.

### Step 3 - Informing Prometheus

Next we need to tell Prometheus that we have some new metrics that we'd like it to utilise.

So lets switch to your operations hat and using what we already have in place (GitOps workflow and ArgoCD) to let prometheus know about a new target to obtain metrics from.

Head over to the [service monitoring documentation](./docs/SERVICEMONITORING.md) for instructions and information on this.

### Step 4 - Custom Grafana Dashboards

Continuing with your operations hat lets visualise those metrics you've added and head over to the [instructions for creating some custom dashboards](./docs/DASHBOARDS.md).

### Step 5 - Tearing things down

To tear things down and shut things off make sure to

* Delete the devops-bookstore-api app using ArgoCD
* Delete the kube-prometheus app using ArgoCD
* Run `terraform destroy`

## FAQ's and Trouble Shooting

**After adding the service monitor definition, my book store is now failing to deploy**

The servicemonitor.yaml file requires a Kubernetes Custom Resource Definition. This gets created when the Prometheus stack is deployed to your cluster. Firstly make sure you have deployed the prometheus stack. Also check the indentation of your YAML files.

**It says my ArgoCD password is incorrect**

There are two ways of obtaining the password. Make sure to try both ways. Also note if you see a '%' sign at the end of the password, this just denotes the end of the line and isn't part of the actual password.









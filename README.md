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




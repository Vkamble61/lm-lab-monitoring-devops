# Observability with Monitoring and Logging 

This repository contains the instructions for getting [Prometheus monitoring](https://prometheus.io/) set up on your cluster.


## Instructions

### Pre-requisites

* You'll need your Kubernetes cluster up and running with ArgoCD deployed to it.

* We'll be making edits to the devops-bookstore-api application so, if you haven't already, you'll need to make sure you have [forked the repository](https://github.com/techreturners/devops-bookstore-api) to your own GitHub account.

* You'll need to refresh yourself with building and deploying (via ArgoCD) your bookstore API to the cluster. For this you can utilise manual building of the docker image on your own machine **or** the CircleCI route as covered in session 5.

### Supporting notes on pre-requisites

In order to get your monitoring setup we're going to have to get your Kubernetes cluster up and running again. So we've got quite a few things to work through first so let's get to it 🚀

* Provisioning cluster, pushing image and utilsing ArgoCD: If you need a reminder on how to provision the cluster and manually push your Docker image, you can follow through the instructions on these links (Just remember to use your own repository for the bookstore-api and you can ignore the tearing down instructions in these links - we'll do that later):
    * Amazon EKS [Instructions](https://github.com/techreturners/lm-lab-eks-terraform-devopsupskill/blob/main/README.md)
    * Google GKE [Instructions](https://github.com/techreturners/devops-upskill-gke-terraform/tree/session-004-gitops#readme)
    * Azure AKS [Instructions](https://github.com/techreturners/devops-upskill-aks-terraform/tree/session-004-gitops#readme)

* (Optional) If you would prefer to utilise CircleCI for the building/pushing of the image part:
    * https://github.com/techreturners/lm-lab-cicd-devops-bookstore-api

From this point onwards the instructions will assume the following:

* Your kubernetes cluster is up and running
* You have configured `kubectl` to be able to talk to your cluster
* Your cluster is running ArgoCD
* You have forked the devops-bookstore-api repository so that you can make changes to the code and push back to your own repository
* You are able to build a docker image of your bookstore API and push back to your container registry (either manually or via CircleCI)
* You know how to port-forward using `kubectl` so that you can access ArgoCD via your browser
* ArgoCD has been configured to be able to deploy your bookstore API

### Monitoring - Deploying the prometheus stack

First we'll get the Prometheus stack deployed to your cluster.

Head over to the [monitoring instructions](./docs/monitoring/INSTRUCTIONS.md) to get prometheus set up and instrumenting your devops-bookstore-api.

### Tearing things down

💡 Don't forget to take any required screenshots before tearing things down

To tear things down and shut things off make sure to

* Delete the devops-bookstore-api app using ArgoCD
* Delete the kube-prometheus app using ArgoCD
* Run `terraform destroy`

## FAQ's and Troubleshooting

**After adding the service monitor definition, my book store is now failing to deploy**

The servicemonitor.yaml file requires a Kubernetes Custom Resource Definition. This gets created when the Prometheus stack is deployed to your cluster. Firstly make sure you have deployed the prometheus stack. Also check the indentation of your YAML files.

**It says my ArgoCD password is incorrect**

There are two ways of obtaining the password. Make sure to try both ways. Also note if you see a '%' sign at the end of the password, this just denotes the end of the line and isn't part of the actual password.









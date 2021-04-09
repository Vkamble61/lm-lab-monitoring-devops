# Introduction

The guide below walks you through how to inform prometheus that there is a new [Target](https://prometheus.io/docs/introduction/glossary/#target) that is exposing metrics for Prometheus to scrape.

To do this we make use of some of the auto-config built into the Prometheus stack we deployed along with a concept of [Service Monitors](https://docs.openshift.com/container-platform/4.7/rest_api/monitoring_apis/servicemonitor-monitoring-coreos-com-v1.html).

Service monitors are created by the CoreOS team and are a Kubernetes [Custom Resource Definition](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/) (CRD)

NOTE: 

All the changes covered in this guide can be seen via this repository comparison.

[https://github.com/techreturners/devops-upskill-gitops/compare/pre-session-005-frontend-deployment...session-006-prometheus](https://github.com/techreturners/devops-upskill-gitops/compare/pre-session-005-frontend-deployment...session-006-prometheus)

Don't forget the changes we'll be covering here should be applied to your forked version of the GitOps repository.

### Step 1 - Labelling the service

In your **devops-upskill-gitops** repository you have the Helm templates for your devops-bookstore-api Kubernetes service.

Lets add some metadata to that service so that we can identify later with a **selector**

In the **devops-bookstore-api/templates/service.yaml** file add the following lines within the metadata section, remembering to be careful with the indentation.

```
labels:
    app: devops-bookstore-api
```

So in full it should now look like this:

```
apiVersion: v1
kind: Service
metadata:
  name: devops-bookstore-api-service
  labels:
    app: devops-bookstore-api
spec:
  type: LoadBalancer
  ports:
  - port: 5000
    targetPort: 5000
    protocol: TCP
    name: http
  selector:
    app: devops-bookstore-api
```

### Step 2 - Introducing a service monitor

Within that same **devops-bookstore-api/templates** directory, create a new file called **servicemonitor.yaml** and put in the following contents:

```
apiVersion: monitoring.coreos.com/v1
kind: ServiceMonitor
metadata:
  name: devops-bookstore-api
  labels:
    release: kube-prometheus-stack #enables prometheus to scrape this monitor
spec:
  selector:
    matchLabels:
      app: devops-bookstore-api
  endpoints:
  - port: http
    interval: 15s
```

Notice how it has a `matchLabels` section - that allows this service monitor to match any services defined with a label of **app** having a value of **devops-bookstore-api**.

Also notice that the service monitor itself has labels of **release** with a value of **kube-prometheus-stack**

This is the auto-magic part of the kubernetes stack you have deployed. It will automatically look for Service Monitors defined with that label. So in turn this informs Prometheus that there is a new target to scrape metrics from and the rest of the config tells it to scrape via a HTTP request every 15 seconds.

By default it will scrape a `/metrics` endpoint, which by default you've exposed on your app when you added that instrumentation.

### Step 3 - Commit and Argo deploy

So now you can commit those changes to your forked GitOps repository.

After a few moments ArgoCD will spot that it is out of Sync. If you click Sync it should then synchronise and re-deploy your application along with the updates to the service labelling and new service monitor.

Now lets head back to the [final part and create some new custom dashboards](./INSTRUCTIONS.md)!




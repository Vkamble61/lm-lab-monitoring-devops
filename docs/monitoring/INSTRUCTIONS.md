# Setting up Prometheus and Monitoring

### Step 1 - Deploying the prometheus stack

First we'll get the Prometheus stack deployed to your cluster.

Head over to the [monitoring documentation](./MONITORING.md) on steps for deploying this stack

### Step 2 - Instrumenting your application

Now you've explored the deployment of Prometheus, lets add some custom monitoring to the backend books API.

In this section imagine you've got your developer hat on and are working on code to make your applications more observable. In turn helping you learn about their performance and your operations teams monitoring of them.

Head over to the [instrumentation documentation](./INSTRUMENTATION.md) for instructions and information on this.

### Step 3 - Informing Prometheus

Next we need to tell Prometheus that we have some new metrics that we'd like it to utilise.

So lets switch to your operations hat and using what we already have in place (GitOps workflow and ArgoCD) to let prometheus know about a new target to obtain metrics from.

Head over to the [service monitoring documentation](./SERVICEMONITORING.md) for instructions and information on this.

### Step 4 - Custom Grafana Dashboards

Continuing with your operations hat lets visualise those metrics you've added and head over to the [instructions for creating some custom dashboards](./DASHBOARDS.md).









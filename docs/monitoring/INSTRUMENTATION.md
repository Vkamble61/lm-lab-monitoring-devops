# Introduction

Prometheus supports a variety of languages and allows a developer to [instrument their code](https://www.overops.com/blog/the-complete-guide-to-instrumentation-how-to-measure-your-application/) with metrics.

It also supports a variety of 3rd party applications such as MySQL, MongoDB, GitHub etc through its [Exporters libraries](https://prometheus.io/docs/instrumenting/exporters/).

The devops-bookstore-api is a Python based application and thankfully there is a [Python Flask Prometheus exporter](https://pypi.org/project/prometheus-flask-exporter/).

The instrumentation we are going to add allows the various [Prometheus metric types](https://prometheus.io/docs/concepts/metric_types/).

NOTE: 

Don't forget the changes we'll be covering here should be applied to your forked version of this repository.

Also all the changes we are making can be seen on this sample repository

https://github.com/techreturners/lm-lab-instrumented-devops-bookstore-api

### Step 1 - Adding the dependency

Firstly add the following line into your Python dependencies in the **Pipfile**. You can add it directly below the line that reads `flask-cors = "*"`

```
prometheus-flask-exporter = "*"
```

### Step 2 - Ensure the pipfile also includes the dependency.

Python manages its dependencies using a combination of the Pipfile and the Pipfile.lock. The lock file locks the versions so we need to also update the lock file.

Replace your current Pipfile.lock with the contents from the sample repository

https://github.com/techreturners/lm-lab-instrumented-devops-bookstore-api/blob/main/Pipfile.lock

### Step 3 - Instrumenting the code

**TIP:** You can see these changes in full on the sample repository

https://github.com/techreturners/lm-lab-instrumented-devops-bookstore-api/blob/main/api.py

Add the required import at the top of the **api.py** file

```
from prometheus_flask_exporter import RESTfulPrometheusMetrics
```

Then after you have instantiated the API (around line 10) you can instantiate the monitoring and record a first metric which will be a static metric informing prometheus of the application name and version. The example hard codes this to 1.4 but you can put in any version.

```
metrics = RESTfulPrometheusMetrics(app, api)

metrics.info('app_info', 'Application info', version='1.0', app_name='devops-bookstore-api')
```

The final step is to decorate the `get` method (which invoked when an API request is made to get books) with some prometheus metrics.

Add the following line above the `get` method:

```
@metrics.summary('requests_by_status', 'Request latencies by status', labels={'status': lambda r: r.status_code})
```

And also include the status code in the response:

```
return {
        "books": [marshal(book, bookFields) for book in books]
    }, 200
```

So it should look like:


```
@metrics.summary('requests_by_status', 'Request latencies by status', labels={'status': lambda r: r.status_code})
def get(self):
    return {
        "books": [marshal(book, bookFields) for book in books]
    }, 200
```

### Step 4 - Commit, Push and Deploy

Now that the code is instrumented you can commit and push your code to your forked repository.

If you have setup CircleCI it will build and push your docker image.

Or alternatively you can push the image up manually - remember to give the image a new tag (version) when you build and push it.

You'll then need to update ArgoCD to deploy the new docker image to your cluster by updating the tag in the **values.yaml** of your GitOps repo.

Remember to utilise ArgoCD to **Synchronize** the state across the cluster.

**TIP:** If you're having trouble accessing the ArgoCD dashboard, sometimes the port-forwarding can timeout. You can Ctrl+C to cancel the existing port-forwarding and then re-run the command.

### Step 5 - Verify that new version is deployed

Once deployed you should be able to get the service address for your backend API

```
kubectl get services
```

And go to http://SERVICE_ADDRESS:5000/metrics to see those Prometheus metrics that you could see in Step 3.

Once verified you can head [back to the next step](./INSTRUCTIONS.md) where we'll work through informing Prometheus about the new metrics your exposing.
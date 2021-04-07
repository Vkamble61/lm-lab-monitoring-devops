# Introduction

Prometheus supports a variety of languages and allows a developer to [instrument their code](https://www.overops.com/blog/the-complete-guide-to-instrumentation-how-to-measure-your-application/) with metrics.

It also supports a variety of 3rd party applications such as MySQL, MongoDB, GitHub etc through its [Exporters libraries](https://prometheus.io/docs/instrumenting/exporters/).

The devops-bookstore-api is a Python based application and thankfully there is a [Python Flask Prometheus exporter](https://pypi.org/project/prometheus-flask-exporter/).

The instrumentation we are going to add allows the various [Prometheus metric types](https://prometheus.io/docs/concepts/metric_types/).

NOTE: 

All the changes covered in this guide can be seen via this repository comparison.

[https://github.com/techreturners/devops-bookstore-api/compare/session-005-cicd...session-006-monitoring](https://github.com/techreturners/devops-bookstore-api/compare/session-005-cicd...session-006-monitoring)

Don't forget the changes we'll be covering here should be applied to your forked version of this repository.

### Step 1 - Adding the dependency

Firstly add the following line into your Python dependencies in the **Pipfile**. You can add it directly below the line that reads `flask-cors = "*"`

```
prometheus-flask-exporter = "*"
```

### Step 2 - Instrumenting the code

Add the required import at the top of the **api.py** file

```
from prometheus_flask_exporter import RESTfulPrometheusMetrics
```

Then after you have instantiated the API (around line 10) you can instantiate the monitoring and record a first metric which will be a static metric informing prometheus of the application name and version. The example hard codes this to 1.4 but you can put in any version.

```
metrics = RESTfulPrometheusMetrics(app, api)

metrics.info('app_info', 'Application info', version='1.0', app_name='devops-bookstore-api')
```

The final step is to decorate the `get` method (which invoked when an API request is made to get books) with some prometheus metrics

```
@metrics.summary('requests_by_status', 'Request latencies by status', labels={'status': lambda r: r.status_code})
def get(self):
    return {
        "books": [marshal(book, bookFields) for book in books]
    }, 200
```

### Step 3 - Testing locally

If you are able to run the backend API locally (by either Docker or the Python command) you should now be able to visit [http://127.0.0.1:5000/metrics](http://127.0.0.1:5000/metrics) and see a list of the Prometheus metrics now being made available for prometheus to scrape.

### Step 4 - Commit, Push and Deploy

Now that the code is instrumented you can commit and push your code to your forked repository.

If you have setup CircleCI it will build and push your docker image.

You'll then need to update ArgoCD to deploy the new docker image to your cluster. (Replacing the old one)

### Step 5 - Verify that new version is deployed

Once deployed you should be able to get the service address for your backend API

```
kubectl get services
```

And go to http://SERVICE_ADDRESS:5000/metrics to see those Prometheus metrics that you could see in Step 3.

Once verified you can head [back to the README](../README.md) where we'll work through informing Prometheus about the new metrics your exposing.
# Introduction

Prometheus supports a variety of languages and allows a developer to [instrument their code](https://www.overops.com/blog/the-complete-guide-to-instrumentation-how-to-measure-your-application/) with metrics.

It also supports a variety of 3rd party applications such as MySQL, MongoDB, GitHub etc through its [Exporters libraries](https://prometheus.io/docs/instrumenting/exporters/).

The devops-bookstore-api is a Python based application and thankfully there is a [Python Flask Prometheus exporter](https://pypi.org/project/prometheus-flask-exporter/).
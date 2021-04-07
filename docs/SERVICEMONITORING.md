# Introduction

The guide below walks you through how to inform prometheus that there is a new [Target](https://prometheus.io/docs/introduction/glossary/#target) that is exposing metrics for Prometheus to scrape.

To do this we make use of some of the auto-config built into the Prometheus stack we deployed along with a concept of [Service Monitors](https://docs.openshift.com/container-platform/4.7/rest_api/monitoring_apis/servicemonitor-monitoring-coreos-com-v1.html).

Service monitors are created by the CoreOS team and are a Kubernetes [Custom Resource Definition](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/) (CRD)


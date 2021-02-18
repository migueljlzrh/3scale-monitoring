Setting up Prometheus

To set up Prometheus, install the Prometheus operator custom resource definition on the cluster and then add Prometheus to an OpenShift project that includes Red Hat 3scale.

Prequisites

* You have system administrator access to the OpenShift cluster.
* You have prepared the OpenShift cluster by installing Red Hat 3scale.

Procedures

1. Login to OpenShift with administrator permissions:
```
$ oc login -u system:admin
```
2. Install the custom resource definitions necessary for running the Prometheus operator.
```
$ oc create -f 3scale-prometheus-crd.yml
```
3. Install the Prometheus operator to your namespace by using the following command syntax.
```
$ oc process -f 3scale-prometheus-operator.yml -p NAMESPACE=<YOUR NAMESPACE> | oc create -f -
```
For example, use this command for a project (namespace) named 3scale-v24:
```
$ oc process -f 3scale-prometheus-operator.yml -p NAMESPACE=3scale-v24 | oc create -f -
```
Note: The first time that you install the Prometheus operator into a namespace, it might take a few minutes for the Prometheus resource pods to start. Subsequently, if you install it to other namespaces on your cluster, the Prometheus resource pods start much faster.

4. Instruct the Prometheus operator to monitor the 3scale's apicast-staging node in a project by using the following command syntax:
```
$ oc process -f 3scale-servicemonitor.yml -p NAMESPACE=<YOUR NAMESPACE> APICAST_STAGING_SERVICE_NAME=apicast-staging | oc apply -f -
```
For example, use this command for an OpenShift project (namespace) named 3scale-v24 that includes 3scale's apicast-staging service.
```
$ oc process -f 3scale-servicemonitor.yml -p NAMESPACE=3scale-v24 APICAST_STAGING_SERVICE_NAME=apicast-staging | oc apply -f -
```

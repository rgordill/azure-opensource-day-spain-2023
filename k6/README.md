# How to deploy K6 in Azure Red Hat Openshift (OCP 4.10)

As it is [described](https://github.com/grafana/k6-operator#deploying-the-operator), clone the github repo and do a make install. It will deploy the operator in `k6-operator-system` namespace, with a cluster-wide approach.

To host the tests, I would do the following:

Create a namespace for the tests (in this case, `k6-tests`).

As there is a [limitation](https://github.com/grafana/k6-operator/issues/160) for running the tests anonymously, that [has been addressed](https://github.com/grafana/k6-operator/pull/202) but no released yet, we need to add a scc to the default ServiceAccount in that project:

```bash
oc adm policy add-scc-to-user nonroot system:serviceaccount:k6-tests:default
```

You may use the official k6 image, or build a custom one. There is an example with the kafka plugin on the container folder.

After that, create the configmaps and k6 instances to launch the tests, as show in kafka folder.
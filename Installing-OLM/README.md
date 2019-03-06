# Install the Operator Lifecycle Manager on MiniShift

### Installation

`oc adm policy add-cluster-role-to-user cluster-admin gfranchini`


Clone the `Operator Lifecycle Manager` repo from GitHub:

```
$ git clone https://github.com/operator-framework/operator-lifecycle-manager
```

Install the required objects:

```
$ oc create -f operator-lifecycle-manager/deploy/okd/manifests/latest/
```

### Verify Installation

Once the installation completes, verify that the operator and all OLM components exist in the `openshift-operator-lifecycle-manager` namespace:

```
$ oc get deploy -n openshift-operator-lifecycle-manager
```

Verify that OLM `CustomResourceDefinitions` are available:

```
$ oc get crds
```

Verify that the `CatalogSource` & `CatalogSource ConfigMap` exist:

```
oc -n openshift-operator-lifecycle-manager get catalogsource
oc -n openshift-operator-lifecycle-manager get configmap
```

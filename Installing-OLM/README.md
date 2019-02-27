# Install the Operator Lifecycle Manager on MiniShift

Clone the `Operator Lifecycle Manager` repo from GitHub:

```
$ git clone https://github.com/operator-framework/operator-lifecycle-manager
```

Install the required objects:

```
$ oc create -f operator-lifecycle-manager/deploy/okd/manifests/0.8.1
```

### Verify Installation

Once the installation completes, verify that the operator and all OLM components exist in the `openshift-operator-lifecycle-manager` namespace.

```
$ oc get deploy -n openshift-operator-lifecycle-manager
```

Verify that OLM `CustomResourceDefinitions` are available:

```
$ oc get crds`
```

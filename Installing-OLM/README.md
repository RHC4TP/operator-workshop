Clone the `Operator Lifecycle Manager` repo from GitHub:

`$ git clone https://github.com/operator-framework/operator-lifecycle-manager`

Install the required objects:

`$ oc create -f operator-lifecycle-manager/deploy/okd/manifests/0.8.1`

Verify that Operators are running under the `openshift-operator-lifecycle-manager` namespace:

`$ oc get deploy -n openshift-operator-lifecycle-manager`

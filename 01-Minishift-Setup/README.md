# Minishift

### Minishift Install

Read the following guide on how to install Minishift on your system:

[Installing Minishift](https://docs.okd.io/latest/minishift/getting-started/installing.html)

## Minishift Start

Run the following commadn to start Minishift on your system. Please ensure you are installing Openshift version `v3.11.0` `(Kubernetes 1.11)`. If you are not using VirtualBox, please provide the vm-driver of your choice (i.e. xhyve, vmwarefusion, hyperkit).

`minishift start --openshift-version v3.11.0 --vm-driver=virtualbox`

## Confirm Minishift is Properly Started

After Minishift has startred, set your `$PATH` to the location of the `oc` binary.

`eval $(minishift oc-env)`

Confirm you are on oc `v3.11.0` or greater.

`oc version`

You should now have the ability to use the `oc` command.

`oc get pods`

Output the current user context.

`oc whoami`

Switch to the `system:admin` user.

`oc login -u system:admin`

Verify you can see all pods in all namespaces.

`oc get pods --all-namespaces`

## Allow Container Images to use root user (Not Recommended in Production)

Verify you are currently scoped to `myproject`.

`oc project myproject`

Create a new Service Account called `useroot`.

`oc create serviceaccount useroot`

Add the `anyuid` Security Context Constraint to the `useroot` Service Account.

`oc adm policy add-scc-to-user anyuid -z useroot`

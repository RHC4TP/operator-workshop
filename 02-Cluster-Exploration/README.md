# Cluster Exploration

## Kubectl / OC Introduction

### Overview
We use the `oc` command to interact with the Kubernetes cluster. The `kubectl` command will also work alongside OpenShift. If you are already familiar with Kubernetes and `kubectl`, you will notice that OpenShift has almost identical commands. View the help to get started.

```bash
oc help
```

### Version
View the Server Version information to verify you can communicate with your Minishift lab environment. Your environment should be on Kubernetes version v1.10.0+ .

```bash
oc version
```

### Nodes
View all of the nodes currently in the cluster (There should only be one since we are working locally).

```bash
oc get nodes
```

Use the `wide` output format option to view more information for each node.

```bash
oc get nodes -o wide
```

View the details for the single Minishift node.

```bash
oc describe node localhost
```

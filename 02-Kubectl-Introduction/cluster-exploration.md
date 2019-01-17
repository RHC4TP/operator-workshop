# Cluster Exploration

## Kubectl Introduction

### Overview
We use the `kubectl` command to interact with the Kubernetes cluster. The `kubectl` command will also work alongside OpenShift. You will notice that OpenShift has almost identical commands, replacing `kubectl` with `oc`. These similarities will be highlighted in the upcoming examples. View help to get started.

`kubectl help`

### Version
View the Server Version information to verify you can communicate with your Minishift lab environment. Your environment should be on Kubernetes version v1.10.0+ .

`kubectl version`

### Cluster
View the basic cluster information.

`kubectl cluster-info`

### Nodes
View all of the nodes currently in the cluster (There should only be one since we are working locally).

`kubectl get nodes`

Use the `wide` output format option to view more information for each node.

`kubectl get nodes -o wide`

View the details for the single Minishift node.

`kubectl describe node localhost`

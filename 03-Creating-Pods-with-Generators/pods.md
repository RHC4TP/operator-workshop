# Pods

## Creating Pods with Generators

### Overview

We use the `run` subcommand to run a Pod, Job, or a Deployment in a cluster. We will discuss Jobs and Deployments later. Use the `--help` option to view the list of available commands.


`kubectl run --help`

Run a simple nginx web server pod.

`kubectl run nginx --image nginx --restart=Never`

Get the list of pods.

`kubectl get pods`

View additional information about the pod.

`kubectl get pods -o wide`

View a detailed description of the running pod.

`kubectl describe pod nginx`


### Running commands in the pod

View the IP address of the running pod.

`kubectl exec nginx -- hostname -i`

Execute an `echo` command within the pod.

`kubectl exec nginx -- /bin/echo "Hello from Redhat"`

Execute an interactive shell session inside the container.

`kubectl exec -it nginx -- /bin/sh`

Run some commands inside the container and exit.
```
ls
cat /etc/os/release
hostname
exit
```

### Clean Up

Use the `delete` subcommand to delete the resources that were created.

`kubectl delete pod nginx`

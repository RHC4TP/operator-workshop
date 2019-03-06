# Pods

## Creating Pods with Generators

### Overview

We use the `run` subcommand to run a Pod, Job, or a Deployment in a cluster. We will discuss Jobs and Deployments later. Use the `--help` option to view the list of available commands.


`oc run --help`

As you can see, the output is quite extensive. You can pipe the stdout to `grep` to get a condensed output.

`oc run --help | grep "restart policy"`

Run a simple nginx web server pod.

`oc run nginx --image nginx --restart=Never`

Get the list of pods.

`oc get pods`

View additional information about the pod.

`oc get pods -o wide`

View a detailed description of the running pod.

`oc describe pod nginx`


### Running commands in the pod

View the IP address of the running pod.

`oc exec nginx -- hostname -i`

Execute an `echo` command within the pod.

`oc exec nginx -- /bin/echo "Hello from Redhat"`

Execute an interactive shell session inside the container.

`oc exec -it nginx -- /bin/sh`

Run some commands inside the container and exit.
```
ls
cat /etc/os/release
hostname
exit
```

Notice the label that automatically gets set on a pod is created with `oc run`.

`oc get pods --show-labels`

### Clean Up

Use the `delete` subcommand to delete the resources that were created.

`oc delete pod nginx`

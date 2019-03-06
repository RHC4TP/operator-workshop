# Pods

## Creating Pods with Generators

### Overview

We use the `run` subcommand to run a Pod, Job, or a Deployment in a cluster. We will discuss Jobs and Deployments later. Use the `--help` option to view the list of available commands.


```bash
oc run --help
```

As you can see, the output is quite extensive. You can pipe the stdout to `grep` to get a condensed output.

```bash
oc run --help | grep "restart policy"
```

Run a simple nginx web server pod.

```bash
oc run nginx --image nginx --restart=Never
```

Get the list of pods.

```bash
oc get pods
```

View additional information about the pod.

```bash
oc get pods -o wide
```

View a detailed description of the running pod.

```bash
oc describe pod nginx
```


### Running commands in the pod

View the IP address of the running pod.

```bash
oc exec nginx -- hostname -i
```

Execute an `echo` command within the pod.

```bash
oc exec nginx -- /bin/echo "Hello from Redhat"
```

Execute an interactive shell session inside the container.

```bash
oc exec -it nginx -- /bin/sh
```

Run some commands inside the container and exit.
```bash
ls
cat /etc/os/release
hostname
exit
```

Notice the label that automatically gets set on a pod is created with `oc run`.

```bash
oc get pods --show-labels
````

### Clean Up

Use the `delete` subcommand to delete the resources that were created.

```bash
oc delete pod nginx
```

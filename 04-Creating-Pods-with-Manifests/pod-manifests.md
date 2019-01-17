# Pods

## Creating Pods with Manifests

### Kubernetes Manifests

Create a new pod manifest that specifies two containers.

```
cat > pod-multi-container.yaml <<EOF
apiVersion: v1
kind: Pod
metadata:
  name: my-two-container-pod
  namespace: myproject
  labels:
    environment: dev
spec:
  containers:
    - name: server
      image: nginx:1.13-alpine
      ports:
        - containerPort: 80
          protocol: TCP
    - name: side-car
      image: alpine:latest
      command: ["/usr/bin/tail", "-f", "/dev/null"]
  restartPolicy: Never
EOF
```
Create the pod by utilizing the manifest created above.

`oc create -f pod-multi-container.yaml`

View the details for the pod.

`oc describe pod my-two-container-pod`


Let's first execute a shell session inside the server container by using the -c flag.

`oc exec -it my-two-container-pod -c server -- /bin/sh`

Run some commands inside the server container.

```
ip address
netstat -tlpn
hostname
ps
exit
```

Let's now execute a shell session inside the side-car container.
`oc exec -it my-two-container-pod -c side-car -- /bin/sh`

Run the same commands in side-car container. Each container within a pod runs its own cgroup, but shares IPC, Network, and UTC (hostname) namespaces.

```
ip address
netstat -tlpn
hostname
exit
```

Verify the currently available api options:
`oc api-versions`

Use the --v flag to set verbosity level. This will allow you to see the request/responses against the Kubernetes API.

`oc got pods --v=8`

### Basic Operations with the Kubernetes api

TODO

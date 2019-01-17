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

`kubectl create -f pod-multi-container.yaml`

View the details for the pod.

`kubectl describe pod my-two-container-pod`


Let's first execute a shell session inside the server container by using the -c flag.

`kubectl exec -it my-two-container-pod -c server -- /bin/sh`

Run some commands inside the server container.

```
ip address
netstat -tlpn
hostname
ps
exit
```

Verify the currently available api options:
`kubectl api-versions`

Use the --v flag to set verbosity level. This will allow you to see the request/responses against the Kubernetes API.

`kubectl got pods --v=8`

### Basic Operations with the Kubernetes api

TODO

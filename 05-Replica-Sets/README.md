# Replica Sets

### Introduction to Replica Sets

Get a list of all pods in the default namespace.

`oc get pods`

Create a ReplicaSet object manifest file.

** NOTE: This example is broken! **
```
cat > replica-set.yaml <<EOF
apiVersion: extensions/v1beta1
kind: ReplicaSet
metadata:
  name: myfirstreplicaset
  namespace: myproject
spec:
  selector:
    matchLabels:
      app: myfirstapp
  replicas: 3
  template:
    metadata:
      labels:
        app: myfirstapp
    spec:
      serviceAccountName: useroot
      containers:
        - name: nodejs
          image: quay.io/coreostrainme/hello-whoami:2.0.1
EOF
```
Create the ReplicaSet.

`oc apply -f replica-set.yaml`

In a new terminal window, select all pods that match app=myfirstapp

`oc get pods -l app=myfirstapp --show-labels -w`

Delete the pod and watch a new one spawn.

`oc delete pod -l app=myfirstapp`

Imperatively scale the ReplicaSet to 6 replicas.

`oc scale replicaset myfirstreplicaset --replicas=6`

Imperatively scale down the ReplicaSet back down to 3 replicas.

`oc scale replicaset myfirstreplicaset --replicas=3`

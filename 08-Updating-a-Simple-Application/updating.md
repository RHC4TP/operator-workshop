# Deployments

## Updating a Simple Application

Scenario: Updating our example application from version 1.0.0 to version 2.0.0

Verify the deployment exists.

```bash
oc get deployment
```

View the current pods

```bash
oc get pods
```

Verify `1.0.0` of our simple app is up by navigating to the OpenShift Route URL.

```bash
oc get route simple-service-route
```


> Note: Open another terminal window and run the following to continually curl the OpenShift Route URL and highlight once our app has been updated to `2.0.0`: `while true; do curl -s $ROUTE_HOST_URL | egrep --color "v2.0.0|$"; sleep 1; done;`

Set the new image for our deployment to `2.0.0`

```bash
oc set image deployment simple-deployment hieveryone=quay.io/coreostrainme/hieveryone:2.0.0 --record=true
```

You should now see a deployment revision number indicating a record of the change.

```bash
oc rollout history deployment simple-deployment
```

Open up your web browser and view the newly updated application.


### Readiness Probes

Create the new deployment object manifest which adds **readiness probes**.

```bash
cat > updated-deployment.yaml<<EOF
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: simple-deployment
  namespace: myproject
  labels:
    k8s-app: simple
spec:
  replicas: 3
  revisionHistoryLimit: 10
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxUnavailable: 0%
      maxSurge: 10%
  template:
    metadata:
      labels:
        k8s-app: simple
    spec:
      serviceAccountName: useroot
      containers:
        - name: hieveryone
          image: quay.io/coreostrainme/hieveryone:1.0.0
          imagePullPolicy: Always
          ports:
            - name: http
              containerPort: 80
          readinessProbe:
           httpGet:
             path: /
             port: 80
             scheme: HTTP
EOF
```
Replace the simple-deployment.

```bash
oc replace -f updated-deployment.yaml
```

Set the image for the deployment to v2.0.0

```bash
oc set image deployment simple-deployment hieveryone=quay.io/coreostrainme/hieveryone:2.0.0 --record=true
```

Readiness probes allow end users to experience seamless updates.

### Clean Up

Let's clean up the Deployment, Service, and Route.

```bash
oc delete deployment simple-deployment
oc delete service simple-service
oc delete route simple-service-route
```

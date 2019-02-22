# Creating an Nginx Pod

## Instructions

1. Write a YAML file called "nginx-deployment.yaml" with the following criteria:

      a.) the resource kind is a Deployment
      
      b.) the name of the resource is "nginx-deployment" and has a label "app: nginx"
      
      c.) The deployment indicates 3 replicas and a selector to match labels for "app: nginx"
      
      d.) The container template also has a label of "app:nginx"
      
      e.) the container image to be pulled is "nginx"
      
      f.) open port 80 so the container can send and accept traffic
      
2. Create the Deployment resource within your cluster
3. Verify the Deployment is running with 3 replica pods, notice a replicaset has been created
4. Check the rollout status of the deployment
5. Delete one pod and watch as a new one is created automatically
6. Delete the deployment 
      
## Solution
 
1.
       controllers/nginx-deployment.yaml
       apiVersion: apps/v1
       kind: Deployment
       metadata:
         name: nginx-deployment
         labels:
           app: nginx
       spec:
         replicas: 3
         selector:
           matchLabels:
             app: nginx
         template:
           metadata:
             labels:
               app: nginx
           spec:
             containers:
             - name: nginx
               image: nginx
               ports:
               - containerPort: 80
2. `oc create -f nginx-deployment.yaml`
3. `oc get deployment`
   `oc get pods`
   `oc get replicaset`
4. `oc rollout status deployment/nginx-deployment`
5. `oc delete pod/<pod-name>`
   `oc get pods`
6. `oc delete -f nginx-deployment.yaml`

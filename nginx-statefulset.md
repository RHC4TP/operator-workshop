# Creating an Nginx Pod

## Instructions

1. Write a YAML file called "nginx-webapp.yaml" with both a service and a stateful set defined within the same file
2. For the service, implement:
     
      a.) the name of the service resource is "nginx-service" and has a label "app: nginx"
      
      b.) specify the port to 80 and give the port a name of "nginx-web"
      
      c.) specify a selector called "app:nginx"
      
3. For the statefulset, implement:
      
      a.) the name of the statefulset resource is "web-app" 
      
      b.) the service name is set to "nginx-service"
      
      c.) there are 6 replicas 
      
      d.) a matchLabels selector set to "app:nginx"
      
      e.) a container template given the label "app: nginx"
      
      f.) container is given a name "nginx" and pulls the image "k8s.gcr.io/nginx-slim:0.8"
      
      g.) container port 80 is open and given a name "nginx-web"

4. In another terminal window, watch the creation of the StatefulSet Pods 
   `oc get pods -w -l app=nginx`

5. Verify the statefulset and service were created

6. Describe the service created and notice the amount of endpoints 

7. Curl one of the endpoints to verify a successful connection to the pods
      
## Solution
 
1,2,3.

    apiVersion: v1
    kind: Service
    metadata:
      name: nginx
      labels:
        app: nginx
    spec:
      ports:
      - port: 80
        name: web
      clusterIP: None
      selector:
        app: nginx
    ---
    apiVersion: apps/v1
    kind: StatefulSet
    metadata:
      name: web
    spec:
      serviceName: "nginx"
      replicas: 2
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
            image: k8s.gcr.io/nginx-slim:0.8
            ports:
            - containerPort: 80
              name: web

4. `oc get pods -w -l app=nginx`
5. `oc get statefulset`
   `oc get service`
6. `oc describe service/<service-name>
7. `curl <endpoint>

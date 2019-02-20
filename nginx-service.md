# Exposing a Service for Nginx Pod

## Instructions

1. Edit your previously create YAML file "nginx-pod.yaml" with the following criteria:

      a.) a label "app: nginx"
      
      b.) expose the container port 80
      
2. Apply the changes made for your Pod to the cluster
3. Create a YAML file named "nginx-service.yaml" with the following criteria:
      a.) the kind is set to Service
      
      b.) a label "app: nginx"
      
      c.) port set to 80 and a protocol of TCP
      
      d.) selector set to "app:nginx"
4. Create the service resouce within the cluster
5. Check that the service is running
6. Describe the service and note the IP address and TargetPort
7. Curl the service's <IP_Address:Port>
      
## Solution
 
1.
       apiVersion: v1
       kind: Pod
       metadata:
         name: nginx-test
       spec:
         volumes:
         - name: data
           emptyDir: {}
         containers:
         - name: nginx
           image: nginx
           volumeMounts:
           - name: data
             mountPath: /usr/share/nginx/html
                  
2. `oc create -f nginx.yaml`
3. `oc get pods`
4. `oc describe pod/nginx-test`
5. `oc exec -it nginx-test -- /bin/bash`
6. `echo Hello World > /usr/share/nginx/html/index.html`
7. `apt-get update`
   `apt-get install curl`
   `curl localhost`

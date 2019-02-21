# Exposing a Service for Nginx Pod

## Instructions

1. Edit your previously created YAML file "nginx-pod.yaml" with the following criteria:

      a.) a label "app: nginx"
      
      b.) expose the container port 80
      
2. Create the Pod
3. Enter the Pod's shell
4. Echo "connection successful" to /usr/share/nginx/html/index.html
5. Exit the shell
6. Create a YAML file named "nginx-service.yaml" with the following criteria:

      a.) the kind is set to Service
      
      b.) a label "app: nginx"
      
      c.) port set to 80 and a protocol of TCP
      
      d.) selector set to "app:nginx"
      
7. Create the service resouce within the cluster
8. Check that the service is running
9. Inspect the service and note the IP address & TargetPort
10. Curl the service's <IP_Address:TargetPort>
11. Delete the resources created
      
## Solution
 
1.
       apiVersion: v1
       kind: Pod
       metadata:
         name: nginx-test
         labels:
           app: nginx
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
           ports:
           - containerPort: 80
                  
2. `oc create -f nginx-pod.yaml`
3. `oc exec -it nginx-test /bin/bash`
4. `echo connection successful > /usr/share/nginx/html/index.html`
5. `exit`
6.
       apiVersion: v1
       kind: Service
       metadata:
         name: nginx-test
         labels:
           app: nginx
       spec:
         ports:
         - port: 80
           protocol: TCP
         selector:
           app: nginx
7. `oc create -f nginx-service.yaml`
8. `oc get service`
9. `oc describe service/nginx-test.yaml`
10. `curl <IP_address:TargetPort>`
11. `kubectl delete -f nginx-pod` `kubectl delete -f nginx-service` 

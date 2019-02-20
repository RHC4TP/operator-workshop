# Exposing a Service for Nginx Pod

## Instructions

1. Edit your previously created YAML file "nginx-pod.yaml" with the following criteria:

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
6. Inspect the service and note the IP address & TargetPort
7. Curl the service's <IP_Address:TargetPort>
      
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
                  
2. `oc apply -f nginx-pod.yaml`
3.
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
4. `oc create nginx-service.yaml`
5. `oc get service`
6. `oc describe service/nginx-service.yaml`
7. `curl <IP_address:TargetPort>`

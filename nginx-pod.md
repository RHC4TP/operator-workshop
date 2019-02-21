# Creating an Nginx Pod

## Instructions

1. Write a YAML file called "nginx-pod.yaml" with the following criteria:

      a.) the resource kind is a Pod
      
      b.) the name of the resource is "nginx-test"
      
      c.) the pod includes an empty volume directory named "data"
      
      d.) the volume directory is mounted at /usr/share/nginx/html
      
      e.) the container image to be pulled is "nginx"
      
2. Create the Pod resource within your cluster
3. Verify the Pod is running
4. Inspect the Pod
5. Enter a shell within the Nginx Pod
6. write "Hello World" to a file located at: /usr/share/nginx/html/index.html
7. send a GET request to the running nginx server. This should outout "Hello World". 

      a.) apt-get update
      
      b.) apt-get install curl
      
      c.) curl localhost
8. Exit the shell
9. Delete the pod created      
      
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
                  
2. `oc create -f nginx-pod.yaml`
3. `oc get pods`
4. `oc describe pod/nginx-test`
5. `oc exec -it nginx-test -- /bin/bash`
6. `echo Hello World > /usr/share/nginx/html/index.html`
7. `apt-get update`
   `apt-get install curl`
   `curl localhost`
   
8. `exit` 
9. `kubectl delete -f nginx-pod.yaml`

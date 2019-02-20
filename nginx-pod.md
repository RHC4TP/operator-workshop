# Creating an Nginx Pod

## Instructions

1. Write a YAML file with the following criteria:

      a.) the resource kind is a Pod
      
      b.) the name of the resource is "nginx-test"
      
      c.) the pod includes an empty volume directory named "data"
      
      d.) the volume directory is mounted at /usr/share/nginx/html
      
      e.) the container image to be pulled is "nginx"
      
2. Create the Pod resource within your cluster
3. Verify the Pod is running
4. Enter a shell within the Nginx Pod
5. write "Hello World" to a file located at: /usr/share/nginx/html/index.html
6. send a GET request to the running nginx server. This should outout "Hello World". 

      a.) apt-get update
      
      b.) apt-get install curl
      
      c.) curl localhost
      
      
## Solution
 

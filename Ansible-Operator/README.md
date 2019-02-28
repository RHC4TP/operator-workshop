# Creating the Memcached Operator (on Minishift)

**Note:** Make sure you are running these commands as an admin. It's also assumed that your MiniShift cluster is already running.

### Create the Operator
1. Use the SDK to create all of the template files for your Operator:
```bash
operator-sdk new awesome-memcached-operator --api-version=cache.example.com/v1alpha1 --kind=Memcached --type=ansible
```

2. Move into your project file:
``` bash
cd awesome-memcached-operator
```

### Build the Operator

Explore the files within the project files and take note of the `roles/` directory. Within are the base directories needed to create an ansible role. In this example, we will be using a pre-defined ansible role from ansible-galaxy.

1. Download the memcached operator role into your `roles/` dir:
```bash
ansible-galaxy install dymurray.memcached_operator_role -p ./roles
```

2. Delete the old templated role:

```bash
rm -rf roles/memcached
```

3. As an example, we will modify the `roles/dymurray.memcached_operator_role/defaults/main.yml` file and change the size to 5. This key-value pair will be interpolated into the `tasks/main.yml` file under `replicas:`

```yml
  ---
  # defaults file for Memcached
  size: 10
```

```yml
---
# tasks file for Memcached
- name: start memcached
  k8s:
    definition:
      kind: Deployment
      apiVersion: apps/v1
      metadata:
        name: '{{ meta.name }}-memcached'
        namespace: '{{ meta.namespace }}'
      spec:
        replicas: "{{size}}"
```
4. Since we are using a role with a name different from the default created by the `operator-sdk` command, we must edit our `watches.yml` file to represent that.

```yml
---
- version: v1alpha1
  group: cache.example.com
  kind: Memcached
  role: /opt/ansible/roles/dymurray.memcached_operator_role # <-- notice me senpai
```

5. Build your docker image using `operator-sdk build` and push to an externally accessible container registry. **Note**: This steps assumes you have made "changes" to your ansible role. By building our container image, we are copying our `role/` dir and `watches.yml` into our ansible container, which is what will be deployed into our k8s cluster.

  * Build
```bash
operator-sdk build gfranchini/awesome-memcached-operator:v0.0.1

INFO[0000] Building Docker image gfranchini/awesome-memcached-operator:v0.0.1
Sending build context to Docker daemon  163.8kB
Step 1/3 : FROM quay.io/operator-framework/ansible-operator:v0.5.0
 ---> 1e857f3522b5
Step 2/3 : COPY roles/ ${HOME}/roles/
 ---> d38c49ac6176
Step 3/3 : COPY watches.yaml ${HOME}/watches.yaml
 ---> 594330e9b6b8
Successfully built 594330e9b6b8
Successfully tagged gfranchini/awesome-memcached-operator:v0.0.1
INFO[0001] Operator build complete.
```
  * Push
```bash
docker push gfranchini/awesome-memcached-operator:v0.0.1
```


### Deploy the Operator
1. Create the Memcached Ansible Operator RBAC and Service Account files

```bash
$ oc create -f deploy/service_account.yaml
$ oc create -f deploy/role.yaml
$ oc create -f deploy/role_binding.yaml

```

2. Create the Memcached Ansible Operator Custom Resource Definitions (CRD)

```bash
oc create -f deploy/crds/cache_v1alpha1_memcached_crd.yaml
```

You can check your CRDS by running:
```bash
oc get crds

NAME                                                                     CREATED AT
catalogsources.operators.coreos.com                                      2019-02-27T20:51:11Z
clusterserviceversions.operators.coreos.com                              2019-02-27T20:51:11Z
gcoperators.myapi.example.com                                            2019-02-28T04:22:54Z
installplans.operators.coreos.com                                        2019-02-27T20:51:11Z
memcacheds.cache.example.com                                             2019-02-28T16:51:54Z
openshiftwebconsoleconfigs.webconsole.operator.openshift.io              2019-02-27T20:36:13Z
operatorgroups.operators.coreos.com                                      2019-02-27T20:51:11Z
servicecertsigneroperatorconfigs.servicecertsigner.config.openshift.io   2019-02-27T20:33:35Z
subscriptions.operators.coreos.com                                       2019-02-27T20:51:11Z
```

3. Edit `deploy/operator.yml` and replace the `image` and `imagePullPolicy` lines with the correct values.

```yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: awesome-memcached-operator
spec:
  replicas: 1
  selector:
    matchLabels:
      name: awesome-memcached-operator
  template:
    metadata:
      labels:
        name: awesome-memcached-operator
    spec:
      serviceAccountName: awesome-memcached-operator
      containers:
        - name: awesome-memcached-operator
          # Replace this with the built image name
          image: gfranchini/awesome-memcached-operator:v0.0.1
          imagePullPolicy: Always
          env:
            - name: WATCH_NAMESPACE
              valueFrom:
                fieldRef:
                  fieldPath: metadata.namespace
            - name: POD_NAME
              valueFrom:
                fieldRef:
                  fieldPath: metadata.name
            - name: OPERATOR_NAME
              value: "awesome-memcached-operator"
```

4. Deploy the Operator:
```bash
oc create -f deploy/operator.yaml
```
Check that the pod is running:

```bash
oc get pods

NAME                                          READY     STATUS    RESTARTS   AGE
awesome-memcached-operator-868869dcb9-tctl8   1/1       Running   0          12s
```

**Note:** This is just the Operator. This is not the cluster we want to run.

5. Create the Memcached Ansible Operator Custom Resource (CR)

```bash
oc create -f deploy/crds/cache_v1alpha1_memcached_cr.yaml
```

Now, run `oc get pods` and you will see our memcached cluster running along side our Operator.

```bash
oc get pods

NAME                                           READY     STATUS    RESTARTS   AGE
awesome-memcached-operator-868869dcb9-tctl8    1/1       Running   0          2m
example-memcached-memcached-6cc844747c-kj4k6   1/1       Running   0          5s
example-memcached-memcached-6cc844747c-mgn9n   1/1       Running   0          5s
example-memcached-memcached-6cc844747c-xmmq5   1/1       Running   0          5s
```

### Clean Up

To uninstall the Deployment and the Memcached Ansible Operator, run the following commands:

```bash
oc delete -f deploy/crds/cache_v1alpha1_memcached_cr.yaml
```
Uninstall Memcached Ansible Operator
```bash
oc delete -f deploy/operator.yaml
```

Verify that the all pods created with the deployment are being terminated and are deleted

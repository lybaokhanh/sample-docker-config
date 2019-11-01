Config file: used to create 'Objects'

Example Object Types:

objects serve different purposes running a container, monitoring a container, setting up networking, etc

- StatefulSet
- ReplicaController
- Pod - runs one or more closely related containers
    1. runs a single set of containers
    2. good for one-off dev purposes
    3. rarely used directly in production

- Deployment: maintains a set of identical pods, ensuring that they have the correct config and that the right number exists

    1. runs a set of identical pods (one or more)
    2. monitors the state of each pod, updating as necessary
    3. good for dev
    4. good for production

- Service - setup networking in a Kubernetes Cluster

Each API version defines a different set of 'objects' we can use

apiVersion: v1

- componentStatus
- configMap
- Endpoints
- Event
- Namespace
- Pod

apiVersion: apps/v1

- ControllerRevision
- StatefulSet

Service config files in depth

--

- Pods: runs one or more closely related containers
- Services: sets up networking in a K8s Cluster
    1. ClusterIP: exposes a container to the inside of k8s Node
    2. NodePort: exposes a container to the outside world (only for dev)
    3. LoadBalancer
    4. Ingress: center point (door gateway) to allow access from the outside world into k8s node
- Secrets: securely stores a piece of information in the cluster, such as a database password

Feed a config file to Kubectl

--
`kubectl apply -f <filename>`

- `kubectl`   CLI we use to change our k8s cluster
- `apply`     change current configuration of our cluster
- `-f`        specific a file that has the config changes

`kubectl get pods`

- `get`     retrieve information about a running object
- `pods`    specifies the object type that we want to get information about

`kubectl config view`

`kubectl describe <object_type> <object_name>`

- eg: `kubectl describe pods client-pod`
- `describe`        get detailed info
- `<object_type>`   specifies the type of object we want to get info about

`kubectl delete -f <config_file>`

- `delete`          delete a running object
- `-f`              specific a file to say what to delete
- `<config_file>`   path to config file that created this object

Imperative command to update image: `kubect set image <object_type>/<object_name> <container_name>=<new_image>`

- eg: `kubectl set image pod/client-pod client=kbly/multi-client` 

`kubectl get storageclass`

Display all persistent volume and persistent volume claim

- `kubectl get pv`
- `kubectl get pvc`

Creating a Secret
`kubectl create secret generic <secret_name> --from-literal key=value`
- eg: `kubectl create secret generic pgpassword --from-literal PG_PASSWORD=123456`
- `create` imperative command to create a new object
- `secret` type of object we're going to create
- `generic` type of secret
- `<secret_name>` name of secret, for later reference in a pod config
- `--from-literal` we're going to add the secret information into this command, as opposed to from file
- `key=value` key-value pair of the secret information 

update pod process:

1. build image again `docker build -t kbly/multi-client:1.0.1 -f Dockerfile.prod .`
2. push image to docker hub `docker push kbly/multi-client:1.0.1`
3. update pod from docker hub image `kubectl set image pod/client-pod client=kbly/multi-client:1.0.1`

Pod config

--
containers, name, port: can't be changes

Persistent Volume Claim:

--
AccessModes:

- ReadWriteOne: can be used by a single node
- ReadOnlyMany: multiple nodes can read from this
- ReadWriteMany: can be read and written to by many nodes

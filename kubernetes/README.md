Q. In kubernetes when I run kubectl apply/create -f deployment.yml may I know what activities will hapeend?
*****************************
When you run kubectl apply -f deployment.yml in Kubernetes, you're instructing Kubernetes to create or update the resources defined in the deployment.yml file.

kubectl interprets the YAML file to identify and understand the Kubernetes resources defined within it, such as Deployments, Services, ConfigMaps, Secrets, PersistentVolumes, and more. 

It then validates the syntax, applies the necessary configurations, kubectl sends the resource definitions to the Kubernetes API server.

The API server authenticates and authorizes the request based on the user's permissions.

The API server validates the resource definitions to ensure they conform to the Kubernetes API specifications.

The API server writes the desired state of the resources to the etcd datastore, which acts as the source of truth for the cluster.

The Controller Manager, a core Kubernetes component, continuously watches the desired state in etcd and compares it to the current state of the cluster.

When it detects a difference, it takes action to align the current state with the desired state.

If new Pods need to be created, the Scheduler assigns them to appropriate nodes based on resource availability and other scheduling constraints.

The Kubelet on each node communicates with the API server to receive instructions on which Pods to run.

The Kubelet ensures the containers for the Pods are started and running as expected.

Containers for the new Pods are pulled from container registries if they aren't already available on the node.

The Kubelet starts the containers using the container runtime (e.g., Docker, containerd).

The Kubelet reports the status of the Pods back to the API server.

The API server updates the status in etcd.

The Controller Manager continues to monitor the state of the resources and ensures they remain aligned with the desired state specified in the deployment.yml file.




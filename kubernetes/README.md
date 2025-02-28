**Q. In kubernetes when I run kubectl apply/create -f deployment.yml may I know what activities will hapeend?**
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

*************
**Q. What is startup probe?**
***************
We are not using any startup probe because most of our applications are built with the latest tag and start quickly. Startup probes are generally used when there is a **need to delay the actual process**, which is more common in legacy, monolithic applications. These applications typically bundle a large amount of business logic and configurations, leading to longer initialization times due to configuration loading and processing.

When running such workloads as Kubernetes pods, the challenge arises when Kubernetes starts routing traffic to the pod as soon as it reaches the "Running" state, even if the application inside is not fully ready to accept requests. While a readiness probe can delay traffic by specifying an initial delay, it does not guarantee that the main application inside the container is ready.

To address this uncertainty, startup probes provide an additional buffer period before readiness and liveness probes take effect. By defining an initial delay (e.g., 1-2 minutes) for startup probes, we can ensure that the container has enough time to initialize before Kubernetes considers it ready. Once the startup probe completes successfully, Kubernetes will begin using the liveness and readiness probes as usual, preventing premature traffic routing and improving application stability.
****
**Q. what is "CrsahLoopBackOff" error in Kubernetes?**
******
The CrashLoopBackOff error occurs in various scenarios, one of the most common being when a non-existent image is referenced in the pod specification. This can happen if the image is missing from the public or private image repository, leading to repeated pod restarts. Each time the pod attempts to pull the image, it fails and enters a restart cycle.

However, Kubernetes does not allow indefinite restarts. Instead, it implements an exponential backoff strategy to manage repeated failures. Initially, if a pod fails, Kubernetes waits 10 seconds before retrying. If it fails again, the wait time increases to 20 seconds, then 40 seconds, and continues exponentially. If the pod remains in a failing state beyond a certain threshold, Kubernetes marks it as CrashLoopBackOff, preventing further restarts.

This behavior applies only when the restart policy is set to Always (the default). If the restart policy is set to Never, the pod will not restart, and the CrashLoopBackOff state will not occur.
***********
**Q. Which monitoring solution you are used and you setup it or consuming?**
*****************
I have used Prometheus and grafana, alert manager with node exporter plugin. The setup is already there, suppose let say some new applications comes are some modifications are needed we do the configuration changes. But the alerts and the SMPTP setings everything slack settings already there. 
*****
Q. Do you know, implementation of prometheus amd grafana form scratch?
*******
Yes, I have experience implementing Alertmanager with Prometheus and Grafana from scratch. I can set up Prometheus for monitoring, configure Alertmanager for notifications, and integrate it with Grafana for visualization.

so the easiest way to use either prometeus operator or prometheous HELM charts, but if you are looking at the various yaml files you should be deploying the deploymnent the statefulsets, the services, the persistent volumes, the service accounts, rbacs, roles, and namespaces if you want to deploy in a particular namespace. so these are the various objects.
****
**Q. How do you scrape HTTPS endpoints in Prometheus?**
******
In Prometheus, by default, scraping happens over HTTP, but if the target exposes metrics over HTTPS, we need to configure it properly. Here's how I would handle it.

In the prometheus.yml file, I set scheme: https under the respective scrape_configs.

If the HTTPS endpoint uses a self-signed certificate, Prometheus will reject it unless we explicitly allow it.
In non-production environments, we can temporarily bypass verification using 
tls_config:
  insecure_skip_verify: true

  But in production, I would use a custom CA certificate:

  tls_config:
  ca_file: /etc/prometheus/certs/ca.crt

If the endpoint requires authentication, we can configure Basic Auth:

basic_auth:
  username: "admin"
  password: "mypassword"

Alternatively, for Bearer Token Authentication:

authorization:
  credentials_file: "/etc/prometheus/auth/token.txt"
  
After updating the config, I restart Prometheus

systemctl restart prometheus  # If running as a system service
docker restart prometheus  # If running in Docker
kubectl rollout restart deployment prometheus -n monitoring  # If running in Kubernetes

Finally, I check if Prometheus is successfully scraping the target via the Prometheus UI (/targets endpoint) or by querying the metrics explorer.
  

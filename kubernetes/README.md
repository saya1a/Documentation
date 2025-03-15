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
The CrashLoopBackOff error occurs in various scenarios, one of the most common being when a **non-existent image** is referenced in the pod specification. This can happen if the image is missing from the public or private image repository, leading to repeated pod restarts. Each time the pod attempts to pull the image, it fails and enters a restart cycle.

However, Kubernetes does not allow indefinite restarts. Instead, it implements an exponential backoff strategy to manage repeated failures. Initially, if a pod fails, Kubernetes waits 10 seconds before retrying. If it fails again, the wait time increases to 20 seconds, then 40 seconds, and continues exponentially. If the pod remains in a failing state beyond a certain threshold, Kubernetes marks it as CrashLoopBackOff, preventing further restarts.

This behavior applies only when the restart policy is set to Always (the default). If the restart policy is set to Never, the pod will not restart, and the CrashLoopBackOff state will not occur.
***********
**Q. Which monitoring solution you are used and you setup it or consuming?**
*****************
I have used Prometheus and grafana, alert manager with node exporter plugin. The setup is already there, suppose let say some new applications comes are some modifications are needed we do the configuration changes. But the alerts and the SMPTP setings everything slack settings already there. 
*****
**Q. Do you know, implementation of prometheus amd grafana form scratch?**
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

****Q. Explain how do you implement the pod disruption budget in kubernets?****

A Pod Disruption Budget (PDB) ensures that a minimum number or percentage of pods remain available during **voluntary disruptions**, such as node maintenance, rolling updates, or cluster autoscaling. This helps in maintaining application availability and preventing downtime.

To implement a PDB, we create a PodDisruptionBudget resource, specifying either **minAvailable** (minimum pods that must stay available) or **maxUnavailable** (maximum pods that can be disrupted) along with a label selector to target specific pods. 

For example, a PDB with minAvailable: 1 ensures that at least one pod is always running. 
We apply the PDB using kubectl apply -f <pdb-file>.yaml and verify it using kubectl get pdb.

PDBs are crucial in high-availability (HA) environments, ensuring applications remain accessible during maintenance activities and rolling updates, particularly for stateful applications like databases. By implementing a PDB, we enhance the resilience of Kubernetes workloads, maintaining service availability while allowing controlled disruptions. 

****Q. can you explain how can you log aggregation in kubernetes?****

Log aggregation in Kubernetes is the process of **collecting, storing, and analyzing logs** from multiple containers, pods, and nodes in a centralized system. Since Kubernetes applications run as distributed microservices across multiple nodes, managing logs efficiently is critical for monitoring, troubleshooting, and auditing.

The need of log aggregation is as the pod is ephemeral nature, since the pods are dynamic and can restart or move between nodes, their logs are not persistent.
also the Applications generates logs inside containers, kubernetes itself logs events, and nodes also generates system logs. instead of manually accessing logs on each node, aggregation allows us to monitor everything in a single place.

kubernetes provides different layers of loggong.

Application logs: logs generated by applications inside container (e.g., stdout and stderr).

Container logs: managed by the container runtime (eg: docker logs <container-id>, or kubectl logs pod>.

Node Logs: System logs from the node (/var/log directory).

Cluster Logs: Logs from Kubernetes components like the API server, scheduler, and controller manager.

there are multiple ways to aggregate the logs in kubernetes
Deploy a sidecar container in the same pod as the application.
The sidecar reads logs from the application container and forwards them to an external system.
Works well for simple setups but does not scale effectively.

Deploy a log collector (e.g., Fluentd, Logstash, Filebeat) as a DaemonSet on all nodes.
The collector reads logs from /var/log/containers/ or /var/log/pods/ and pushes them to an external logging system.

Kubernetes Logging Stack (EFK / ELK)
Elasticsearch + Fluentd + Kibana (EFK Stack):
Fluentd collects logs and sends them to Elasticsearch.
Kibana provides visualization and analysis.
Elasticsearch + Logstash + Kibana (ELK Stack):
Logstash is used instead of Fluentd for more advanced log processing.

**Q. what liveness and readiness probes in Kubernetes?**

In Kubernetes, liveness and readiness probes are used to monitor and manage the health of containerized applications. These probes ensure that the application remains reliable even during container failures. By using these probes, Kubernetes can prevent traffic from being sent to unhealthy containers that are not ready to serve, thereby maintaining the overall reliability and stability of the application.

Liveness probes in Kubernetes check the specified endpoint's health status by targeting the defined health path and port. If the response is 200, the container is marked as healthy. Otherwise, Kubernetes restarts the container helping it recover automatically. The first check occurs 10 seconds after the container starts, as specified by initialDelaySeconds, and subsequent checks occur every 5 seconds, as defined by periodSeconds.

Imagine a Java-based microservice running inside a Kubernetes pod. Due to a memory leak or deadlock, the application becomes unresponsive but does not crash. The process is still running, but it no longer serves requests.

Use Case: Prevents unresponsive applications from consuming resources indefinitely.

Suppose a Node.js application connects to a database before serving traffic. When the pod starts, the database connection takes 30 seconds to establish. If the application receives requests before the connection is ready, users will see errors.

A readiness probe ensures the application is ready to handle traffic. Until the probe passes, Kubernetes does not send traffic to the pod.

Use Case: Ensures applications receive traffic only when fully ready.

**Q. What is Service Discovery in Kubernetes?**

Service discovery in kubernetes is the mechanism by which applications automatically detect and communicate with other services within a cluster. Since pods in Kubernetes are ephemeral and their IPs changes frequentely. Service discivery ensures that applications can dynamically locate and interact with servies without requiring manul configuration.

Kubernetes provides two primary ways to discover services.

1. DNS-Based Service Discovery
2. environment variable-based discovery

The most common method is DNS-based discovery, where Kubernetes uses CoreDNS (or Kube-DNS) to resolve service names into stable, internal cluster IPs. Each service in Kubernetes is automatically assigned a DNS name in the format _servicename.namespace.svc.cluster.local_, allowing applications to reference other services using human-readable names instead of IP addresses. 

For example, a frontend application can connect to a backend service using _http://backend.default.svc.cluster.local_

Another approach is environment variable-based discovery, where Kubernetes **injects environment variables** into each Pod, containing service details such as IP and port. However, this method is static and only works for services created before the Pod starts.

For example, a database service may have environment variables like DATABASE_SERVICE_HOST=10.0.0.5 and DATABASE_SERVICE_PORT=5432, which an application can use to establish a connection


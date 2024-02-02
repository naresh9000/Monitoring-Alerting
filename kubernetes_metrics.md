#          Kubernetes Metrics

Pod CPU Usage:

Monitor CPU usage of Kubernetes pods.
Example query: sum(rate(container_cpu_usage_seconds_total{namespace=~"namespace.*"}[5m])) by (pod)
Pod Memory Usage:

Monitor memory usage of Kubernetes pods.
Example query: sum(container_memory_usage_bytes{namespace=~"namespace.*"}) by (pod)
Pod Network Traffic:

Monitor inbound and outbound network traffic of Kubernetes pods.
Example query: sum(rate(container_network_receive_bytes_total{namespace=~"namespace.*"}[5m])) by (pod)
Pod Restart Count:

Monitor the number of times Kubernetes pods have been restarted.
Example query: kube_pod_container_status_restarts_total{namespace=~"namespace.*"}
Kubernetes Cluster Resource Usage:

Monitor overall CPU and memory usage of the Kubernetes cluster.
Example query: sum(container_memory_usage_bytes) / sum(machine_memory_bytes) * 100 (for memory usage)
Kubernetes API Server Latency:

Monitor latency of Kubernetes API server requests.
Example query: sum(rate(kube_apiserver_request_duration_seconds_bucket{job="apiserver", le="0.5"}[5m]))


Pod CPU Throttling:

Monitor CPU throttling events experienced by Kubernetes pods.
Example query: sum by (pod) (rate(container_cpu_cfs_throttled_periods_total{namespace=~"namespace.*"}[5m]))
Pod Memory Page Faults:

Monitor memory page faults experienced by Kubernetes pods.
Example query: sum(container_memory_failcnt{namespace=~"namespace.*"}) by (pod)
Kubernetes API Server Throughput:

Monitor the throughput of Kubernetes API server requests.
Example query: sum(rate(kube_apiserver_request_count[5m]))
Kubernetes Controller Manager Metrics:

Monitor the performance and operation of the Kubernetes controller manager.
Example query: kube_controller_manager_runtime_leader_election_lease_duration_seconds
Kubernetes Scheduler Metrics:

Monitor the performance and operation of the Kubernetes scheduler.
Example query: kube_scheduler_election_duration_seconds
Kubernetes Node Status:

Monitor the status of Kubernetes nodes (e.g., ready, not ready).
Example query: kube_node_status_condition{condition="Ready", status="true"}
Kubernetes Pod Status:

Monitor the status of Kubernetes pods (e.g., running, pending, failed).
Example query: kube_pod_status_phase{phase="Running"}
Kubernetes Event Rate:

Monitor the rate of Kubernetes events generated by the API server.
Example query: rate(kube_events_total[5m])
Kubernetes Resource Quota Utilization:

Monitor the utilization of resource quotas defined for Kubernetes namespaces.
Example query: kube_resourcequota{namespace="namespace_name", type="used"}
Kubernetes Horizontal Pod Autoscaler Metrics:

Monitor the behavior and performance of Horizontal Pod Autoscaler (HPA) objects.
Example query: kube_horizontalpodautoscaler_spec_max_replicas
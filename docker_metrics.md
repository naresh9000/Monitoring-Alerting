#         Docker Metrics


Container CPU Usage:

Monitor CPU usage of Docker containers.
Example query: container_cpu_usage_seconds_total{container_name=~"container.*"}
Container Memory Usage:

Monitor memory usage of Docker containers.
Example query: container_memory_usage_bytes{container_name=~"container.*"}
Container Network Traffic:

Monitor inbound and outbound network traffic of Docker containers.
Example query: rate(container_network_receive_bytes_total{container_name=~"container.*"}[5m])
Container Restart Count:

Monitor the number of times Docker containers have been restarted.
Example query: container_restart_count{container_name=~"container.*"}


Container Disk I/O:

Monitor read and write operations on Docker container disks.
Example query: rate(container_fs_reads_bytes_total{container_name=~"container.*"}[5m]) (for reads) and rate(container_fs_writes_bytes_total{container_name=~"container.*"}[5m]) (for writes)
Container Filesystem Usage:

Monitor filesystem usage of Docker containers.
Example query: container_fs_usage_bytes{container_name=~"container.*"}
Container Network Errors:

Monitor network errors and failures encountered by Docker containers.
Example query: rate(container_network_errors_total{container_name=~"container.*"}[5m])
Container CPU Throttling:

Monitor CPU throttling events experienced by Docker containers.
Example query: container_cpu_cfs_throttled_periods_total{container_name=~"container.*"}
Container Memory Page Faults:

Monitor memory page faults experienced by Docker containers.
Example query: container_memory_failcnt{container_name=~"container.*"}
Container Health Check Status:

Monitor the status of health checks configured for Docker containers.
Example query: container_healthcheck_status{container_name=~"container.*"}
Container Network Latency:

Monitor network latency experienced by Docker containers.
Example query: container_network_tcp_latency_seconds{container_name=~"container.*"}
Container CPU Utilization by Core:

Monitor CPU utilization of Docker containers per CPU core.
Example query: container_cpu_usage_seconds_total{container_name=~"container.*"} / count without (cpu)(irate(container_cpu_usage_seconds_total{container_name=~"container.*"}[5m]))
Container Memory Swap Usage:

Monitor memory swap usage of Docker containers.
Example query: container_memory_swap{container_name=~"container.*"}
Container CPU Throttling Time:

Monitor the amount of time Docker containers spend CPU-throttled.
Example query: sum by (container_name) (rate(container_cpu_cfs_throttled_seconds_total{container_name=~"container.*"}[5m]))
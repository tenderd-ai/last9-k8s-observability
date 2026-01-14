# Kubernetes Metrics Filtering Configuration

## Overview
We have updated the monitoring configuration to strictly filter the metrics sent to Last9, ensuring only the essential 36 metrics (plus `up`) are collected. This significantly reduces cardinality and cost while maintaining observability for Deployments, Pods, and StatefulSets.

## Configuration Change
The filtering is applied at the **Prometheus Remote Write** level in `k8s-monitoring-values.yaml`. This ensures that while the cluster might scrape various metrics, only the allowed metrics are transmitted to the backend.

### `writeRelabelConfigs`
We use `writeRelabelConfigs` to drop all metrics that do not match our allowlist regex.

```yaml
writeRelabelConfigs:
  - sourceLabels: [__name__]
    regex: "up|kube_deployment_spec_replicas|..." # (Full regex below)
    action: keep
```

## Allowed Metrics List

The following metrics are preserved:

### Deployment Metrics
- `kube_deployment_spec_replicas`
- `kube_deployment_status_replicas_ready`
- `kube_deployment_status_replicas_updated`
- `kube_deployment_status_replicas_available`
- `kube_deployment_status_replicas_unavailable`

### Pod Metrics
- `kube_pod_status_phase`
- `kube_pod_created`
- `kube_pod_container_status_running`
- `kube_pod_container_info`
- `kube_pod_container_status_restarts_total`
- `kube_pod_info`
- `kube_pod_owner`
- `kube_pod_container_status_waiting_reason`
- `kube_pod_container_status_terminated`
- `kube_pod_container_status_last_terminated_reason`
- `kube_pod_container_status_terminated_reason`

### StatefulSet Metrics
- `kube_statefulset_created`
- `kube_statefulset_replicas`
- `kube_statefulset_status_replicas_ready`
- `kube_statefulset_status_replicas_updated`
- `kube_statefulset_status_replicas_available`
- `kube_statefulset_status_replicas_unavailable`
- `kube_statefulset_spec_replicas`
- `kube_statefulset_info`

### Resource & Container Metrics
- `kube_pod_container_resource_requests`
- `kube_pod_container_resource_limits`
- `container_cpu_usage_seconds_total`
- `container_memory_working_set_bytes`
- `container_memory_usage_bytes`
- `container_fs_reads_bytes_total`
- `container_fs_writes_bytes_total`
- `container_network_receive_bytes_total`
- `container_network_transmit_bytes_total`
- `container_network_receive_errors_total`
- `container_network_transmit_errors_total`
- `container_cpu_cfs_throttled_periods_total`

### System Metrics
- `up` (Required for target health monitoring)

## How to Apply
To apply these changes, run the setup script with the monitoring-only or full mode:

```bash
./last9-otel-setup.sh monitoring-only \
  monitoring-endpoint="https://your-metrics-endpoint" \
  username="<user>" \
  password="<pass>"
```

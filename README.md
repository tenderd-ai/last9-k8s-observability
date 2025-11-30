# Last9 OpenTelemetry Operator Setup

Automated setup script for deploying OpenTelemetry Operator, Collector, Kubernetes monitoring, and Events collection to your Kubernetes cluster with Last9 integration.

## Features

- ✅ **One-command installation** - Deploy everything with a single command
- ✅ **Flexible deployment options** - Install only what you need (logs, traces, metrics, events)
- ✅ **Auto-instrumentation** - Automatic instrumentation for Java, Python, Node.js, and more
- ✅ **Kubernetes monitoring** - Full cluster observability with kube-prometheus-stack
- ✅ **Events collection** - Capture and forward Kubernetes events
- ✅ **Cluster identification** - Automatic cluster name detection and attribution
- ✅ **Tolerations support** - Deploy on tainted nodes (control-plane, spot instances, etc.)
- ✅ **Environment customization** - Override deployment environment and cluster name

## Quick Start

### Prerequisites

- `kubectl` configured to access your Kubernetes cluster
- `helm` (v3+) installed

### Option 1: Install Everything (Recommended)

Installs OpenTelemetry Operator, Collector, Kubernetes monitoring stack, and Events agent:

```bash
./last9-otel-setup.sh \
  token="Basic <your-base64-token>" \
  endpoint="<your-otlp-endpoint>" \
  monitoring-endpoint="<your-metrics-endpoint>" \
  username="<your-username>" \
  password="<your-password>"
```

### Quick Install (One-liner)

```bash
curl -fsSL https://raw.githubusercontent.com/last9/l9-otel-operator/main/last9-otel-setup.sh | bash -s -- \
  token="Basic <your-token>" \
  endpoint="<your-otlp-endpoint>" \
  monitoring-endpoint="<your-metrics-endpoint>" \
  username="<user>" \
  password="<pass>"
```

## Installation Options

### Option 2: Traces Only (Operator + Collector)

For applications that need distributed tracing:

```bash
./last9-otel-setup.sh operator-only \
  token="Basic <your-token>" \
  endpoint="<your-otlp-endpoint>"
```

### Option 3: Logs Only (Collector without Operator)

For log collection use cases:

```bash
./last9-otel-setup.sh logs-only \
  token="Basic <your-token>" \
  endpoint="<your-otlp-endpoint>"
```

### Option 4: Metrics Only (Kubernetes Monitoring)

For cluster metrics and monitoring:

```bash
./last9-otel-setup.sh monitoring-only \
  monitoring-endpoint="<your-metrics-endpoint>" \
  username="<your-username>" \
  password="<your-password>"
```

### Option 5: Kubernetes Events Only

For Kubernetes events collection:

```bash
./last9-otel-setup.sh events-only \
  endpoint="<your-otlp-endpoint>" \
  token="Basic <your-base64-token>" \
  monitoring-endpoint="<your-metrics-endpoint>"
```

## Advanced Configuration

### Override Cluster Name

```bash
./last9-otel-setup.sh \
  token="..." \
  endpoint="..." \
  cluster="prod-us-east-1"
```

If not provided, the cluster name is auto-detected from `kubectl config current-context`.

### Set Deployment Environment

```bash
./last9-otel-setup.sh \
  token="..." \
  endpoint="..." \
  env="production"
```

Default: `staging` for collector, `local` for auto-instrumentation.

### Deploy with Tolerations

For deploying on nodes with taints (e.g., control-plane, monitoring nodes):

```bash
./last9-otel-setup.sh \
  token="..." \
  endpoint="..." \
  tolerations-file=/path/to/tolerations.yaml
```

**Example tolerations files** are provided in the `examples/` directory:
- `tolerations-all-nodes.yaml` - Deploy on all nodes including control-plane
- `tolerations-monitoring-nodes.yaml` - Deploy on dedicated monitoring nodes
- `tolerations-spot-instances.yaml` - Deploy on spot/preemptible instances
- `tolerations-multi-taint.yaml` - Handle multiple taints
- `tolerations-nodeSelector-only.yaml` - Use nodeSelector without tolerations

## Configuration Files

| File | Description |
|------|-------------|
| `last9-otel-collector-values.yaml` | OpenTelemetry Collector configuration for logs and traces |
| `k8s-monitoring-values.yaml` | Kube-prometheus-stack configuration for metrics |
| `last9-kube-events-agent-values.yaml` | Events collection agent configuration |
| `collector-svc.yaml` | Collector service for application instrumentation |
| `instrumentation.yaml` | Auto-instrumentation configuration |
| `deploy.yaml` | Sample application deployment with auto-instrumentation |
| `tolerations.yaml` | Sample tolerations configuration |

### Placeholders

The following placeholders are automatically replaced during installation:

- `{{AUTH_TOKEN}}` - Your Last9 authorization token
- `{{OTEL_ENDPOINT}}` - Your OTEL endpoint URL
- `{{MONITORING_ENDPOINT}}` - Your metrics endpoint URL

## Uninstallation

### Uninstall Everything

```bash
./last9-otel-setup.sh uninstall-all
```

### Uninstall Specific Components

```bash
# Uninstall only monitoring stack
./last9-otel-setup.sh uninstall function="uninstall_last9_monitoring"

# Uninstall only events agent
./last9-otel-setup.sh uninstall function="uninstall_events_agent"

# Uninstall OpenTelemetry components (operator + collector)
./last9-otel-setup.sh uninstall
```

## Verification

After installation, verify the deployment:

```bash
# Check all pods in last9 namespace
kubectl get pods -n last9

# Check collector logs
kubectl logs -n last9 -l app.kubernetes.io/name=opentelemetry-collector

# Check monitoring stack
kubectl get prometheus -n last9

# Check events agent
kubectl get pods -n last9 -l app.kubernetes.io/name=last9-kube-events-agent
```

## Auto-Instrumentation

The script automatically sets up instrumentation for:

- ☕ **Java** - Automatic OTLP export
- 🐍 **Python** - Automatic OTLP export
- 🟢 **Node.js** - Automatic OTLP export
- 🔵 **Go** - Manual instrumentation supported
- 💎 **Ruby** - Coming soon

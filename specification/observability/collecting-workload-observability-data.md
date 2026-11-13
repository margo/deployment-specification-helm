# Collecting Workload Observability Data Supplemental Material

The following content supplements the [Collecting Workload Observability Data](https://docs.margo.org/specification/observability/collecting-workload-observability-data) documentation in the core specification with requirements clarifications specific to the Helm deployment technology.

## Kubernetes Platform Observability Requirements

For devices running Kubernetes, the following is a minimum list of observability data that MUST be provided. The device owner MAY choose to provide additional observability data if they wish.

- Cluster (single- or multi-node) observability data MUST be collected.
  - It is recommended that the Device Owner use the [Kubernetes Cluster Receiver](https://opentelemetry.io/docs/kubernetes/collector/components/#kubernetes-cluster-receiver) with the default configuration to collect this information, but using this receiver is not required.
  - If the Device Owner chooses not to use the Kubernetes Cluster Receiver, they MUST provide the same output as the Kubernetes Cluster Receiver's default configuration.

  > **Note:** Please see the [information below](#workload-observability-default-telemetry) for the default metrics emitted by the Kubernetes Cluster Receiver.

- Cluster events observability data MUST be collected.
  - It is recommended that the Device Owner use either the [Kubernetes Objects Receiver](https://opentelemetry.io/docs/kubernetes/collector/components/#kubernetes-objects-receiver) or [Kubernetes Events Receiver](https://github.com/open-telemetry/opentelemetry-collector-contrib/blob/main/receiver/k8seventsreceiver/README.md) with the default configuration to collect this information, but using either of these receivers is not required.
  - If the Device Owner chooses not to use either the Kubernetes Object Receiver or Kubernetes Events Receiver, they MUST provide the same output as the Kubernetes Events Receiver's default configuration.

> **Action:** Need to determine which namespaces should be included. All of them, or just the ones the device owner is responsible for creating.
>
> **Action:** The Kubernetes objects receiver needs to be configured to export events and other resource logs, so we'll need to document something additional for this receiver.

- Node, Pod, and Container observability data MUST be collected.
  - It is recommended that the Device Owner use the [Kubelet Stats Receiver](https://opentelemetry.io/docs/kubernetes/collector/components/#kubeletstats-receiver) with the default configuration to collect this information, but using this receiver is not required.
  - If the Device Owner chooses not to use the Kubelet Stats Receiver, they MUST provide the same output as the Kubelet Stats Receiver's default configuration.

  > **Note:** Please see the [information below](#workload-observability-default-telemetry) for the default metrics emitted by the Kubelet Stats Receiver.

- Metadata identifying the observability data's source MUST be added to the received observability data.
  - It is recommended that the Device Owner use the [Kubernetes Attributes Processor](https://opentelemetry.io/docs/kubernetes/collector/components/#kubernetes-attributes-processor) with the default configuration to enhance the observability data with this additional metadata, but using this processor is not required.
  - If the Device Owner chooses not to use the Kubernetes Attributes Processor, they MUST provide the same metadata as the Kubernetes Attributes Processor's default configuration

  > **Note:** Please see the [information below](#workload-observability-default-telemetry) for the default attributes added by the Kubernetes Attributes Processor.

## Workload Observability Default Telemetry

The following telemetry data is collected using the default configurations for the receivers indicated above. You can find more information about each piece of telemetry in the receiver's documentation.

### Metrics

The following table shows the metrics emitted by the indicated receivers when using the default configuration.

| Metric Group   |  Metric  | Target | [Kubernetes Cluster Receiver](https://github.com/open-telemetry/opentelemetry-collector-contrib/tree/main/receiver/k8sclusterreceiver) |  [Kubelet Stats Receiver](https://github.com/open-telemetry/opentelemetry-collector-contrib/tree/main/receiver/kubeletstatsreceiver) | [Host Metrics Receiver](https://github.com/open-telemetry/opentelemetry-collector-contrib/tree/main/receiver/hostmetricsreceiver) |
|----------|---------|-------|----------------|--------------------------|-------------------------|
| CPU | Limit | Container | X |  |   |
| CPU | Load Average (15m, 5m, 1m) | System |  |  | X |
| CPU | Time | Container, Kubernetes Node, Kubernetes Pod, System |  | X | X |
| CPU | Request | Container | X |  |  |
| CPU | Usage Kernel Mode | Container |   |  |  |
| CPU | Utilization | Container, Kubernetes Node, Kubernetes Pod |  | X |  |
| Disk | IO | Container, System |  |  | X |
| File System | Available | Container, Kubernetes Node, Kubernetes Pod |  | X |  |
| File System | Capacity | Container, Kubernetes Node, Kubernetes Pod  |  | X |  |
| File System | Inodes | Kubernetes Volume, System |  | X |  |
| File System | Inodes Free | Volume |  | X |  |
| File System | Inodes Used | Volume |  | X |  |
| File System | Usage | Container, Kubernetes Node, Kubernetes Pod, System|  | X |  X |
| Memory | Available | Container, Kubernetes Node, Kubernetes Pod  |  | X |  |
| Memory | Limit | Container | X |  |  |
| Memory | Major Page Fault | Container, Kubernetes Node, Kubernetes Pod |  | X |  |
| Memory | Page Faults | Container, Kubernetes Node, Kubernetes Pod |  | X |
| Memory | Request | Container | X |  |  |
| Memory | RSS | Container, Kubernetes Node, Kubernetes Pod |  | X |  |
| Memory | Usage | Container, Kubernetes Node, Kubernetes Pod, System |  | X | X |
| Memory | Working Set | Container, Kubernetes Node, Kubernetes Pod |  | X |  |
| Network | Connections | System |  |  | X |
| Network | Errors | Kubernetes Node, Kubernetes Pod, System |  | X | X |
| Network | IO | Kubernetes Node, Kubernetes Pod, System |  | X | X |
| Network | IO Packets | System |  |  | X |
| Network | IO Packets Dropped | System |  |  | X |
| Paging | Faults | System |  |  | X |
| Paging | Operations | System |  |  | X |
| Paging | Usage | System |  |  | X |
| Process | CPU Time | System |  |  | X |
| Process | Disk IO | System |  |  | X |
| Processes | Count | System |  |  | X |
| Processes | Created | System |  |  | X |
| Resource Quota | Hard Limit | Various | X |  |  |
| Resource Quota | Used | Various | X |  |  |
| State | Ready | Container | X |  |  |
| State | Restarts | Container | X |  |
| State | Active Jobs | Cron Job | X |  |
| State | Current Scheduled Nodes | Daemonset | X |  |
| State | Desired Scheduled Nodes | Daemonset | X |  |
| State | Misscheduled Modes | Daemonset | X |  |
| State | Ready Nodes | Daemonset | X |  |
| State | Available | Deployment | X |  |
| State | Desired | Deployment | X |  |
| State | Current Replicas | HPA | X |  |
| State | Desired Replicas | HPA | X |  |
| State | Max Replicas | HPA | X |  |  |
| State | Min Replicas | HPA | X |  |  |
| State | Active Pods | Job | X |  |  |
| State | Desired Successful Pods | Job | X |  |  |
| State | Failed Pods | Job | X  |  |  |
| State | Max Parallel Jobs | Job | X |  |  |
| State | Successful Pods | Job | X |  |  |
| State | Phase | Namespace | X |  |  |
| State | Phase | Pod | X |  |  |
| State | Available | Replicaset | X |  |  |
| State | Desired | Replicaset | X |  |  |
| State | Available | Replication Controller | X |  |  |
| State | Desired | Replication Controller | X |  |  |
| State | Current Pods | Stateful Set | X |  |  |
| State | Desired Pods | Stateful Set | X |  |  |
| State | Ready Pods | Stateful Set | X |  |  |
| State | Updated Pods | Stateful Set | X |  |
| Storage | Available | Volume |  | X |  |
| Storage | Capacity | Volume |  | X |  |
| Storage | Limit | Container | X |  |  |
| Storage (Ephemeral) | Limit | Container | X |  |  |
| Storage | Requests | Container | X |  |  |
| Storage (Ephemeral) | Request | Container | X |  |  |

### Logs

The Kubernetes Events receiver collects the event logs when using the default configuration. The Kubernetes Object Receiver must be configured to collect the desired logs. Container logs must be emitted using OTLP.

### Kubernetes Attributes Processor

The following shows the attributes added to each signal when using the [Kubernetes Attribute Processors](https://github.com/open-telemetry/opentelemetry-collector-contrib/tree/main/processor/k8sattributesprocessor) default configuration.

- k8s.namespace.name
- k8s.pod.name
- k8s.pod.uid
- k8s.pod.start_time
- k8s.deployment.name
- k8s.node.name

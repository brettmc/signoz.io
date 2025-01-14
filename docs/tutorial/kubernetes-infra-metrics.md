---
id: kubernetes-infra-metrics
title: K8s Infra Metrics and Logs Collection
description: View Kubernetes infrastructure metrics and logs in SigNoz
---

import K8sMetrics from '../shared/k8s-metrics-list.md'
import HostMetrics from '../shared/hostmetrics-list.md'

### Overview
To export K8s metrics, you can enable different receivers in OpenTelemetry
collector which will send metrics about your Kubernetes (K8s) infrastructure
to SigNoz. These OpenTelemetry collectors will act as agents which send
metrics about K8s to SigNoz.

OtelCollector agent can also be used to tail and parse logs generated by
container using `filelog` receiver and send it to desired receiver.

Based on where you are running SigNoz ( e.g. in an independent VM or K8s cluster),
you have to provide the address to send data from the above receivers.

## Steps to export K8s metrics to SigNoz

By default, SigNoz Helm chart installs `k8s-infra` dependency chart which
handles the task of collecting metrics and logs from the K8s cluster.

`K8s-Infra` helm chart mainly handles the following:
- Tails and parses logs generated by containers in K8s cluster and sends to SigNoz
- Collects kubelet metrics and host metrics from each nodes of the K8s cluster
- Collects cluster-level metrics from the Kubernetes API server
- Acts as a gateway to send any incoming OTLP telemetry data to SigNoz OtelCollector

:::info
Skip **Step 1** if you have single K8s cluster for SigNoz and your applications
as `k8s-infra` chart is included in default SigNoz chart installation.
:::

1. **Install K8s-Infra chart**

   ```bash
   helm install my-release signoz/k8s-infra  \
     --set otelCollectorEndpoint=<IP-or-Endpoint-of-SigNoz-OtelCollector>:4317
   ```

   :::info
   If the OtelCollector endpoint is secured, you would have to enable `otelInsecure`
   configuration and often make other changes such as including either config
   or path to the TLS certificate and private key.

   In case of SigNoz Cloud, you would have to set `signozApiKey` configuration.
   :::

2. **Plot Metrics in SigNoz UI**

   To plot metrics generated from `k8s-infra` chart, follow the instructions given
   in the docs [here][4].

   Check out the [List of metrics from Kubernetes receiver][3].

3. **Import Dashboard with CPU and Memory Metrics**

   You can import basic dashboard with CPU and Memory metrics of K8s cluster
   containers from [here][5].

   After importing, you can include more widgets using other metrics from
   the [list below][3].

4. **Generate and Import Dashboard with Node Metrics**
   
   To generate node metrics dashboards for each nodes of K8s cluster:

   ```bash
   for node in $(kubectl get nodes -o name | sed -e "s/^node\///");
   do
   curl -sL https://github.com/SigNoz/benchmark/raw/main/dashboards/hostmetrics/hostmetrics-import.sh \
      | HOSTNAME="$node" DASHBOARD_TITLE="Node Metrics Dashboard for $node" bash
   done
   ```

   After importing the generated dashboards, you can include more widgets as
   per you need using metrics from the [list below][3].

---

### List of metrics

<K8sMetrics />

<HostMetrics name="Node Metrics"/>

---

[1]: https://github.com/SigNoz/otel-collector-k8s/blob/main/agent/infra-metrics.yaml#L47
[2]: https://github.com/SigNoz/otel-collector-k8s/blob/main/deployment/all-in-one.yaml#L19
[3]: #list-of-metrics
[4]: https://signoz.io/docs/userguide/dashboards/
[5]: https://github.com/SigNoz/benchmark/raw/main/dashboards/k8s-infra-metrics/cpu-memory-metrics.json

<!--- GENERATED BY gomplate from scripts/docs/templates/monitor-page.md.tmpl --->

# prometheus/velero

Monitor Type: `prometheus/velero` ([Source](https://github.com/signalfx/signalfx-agent/tree/main/pkg/monitors/prometheus/velero))

**Accepts Endpoints**: **Yes**

**Multiple Instances Allowed**: Yes

## Overview

This monitor gets metrics from 
[Velero](https://github.com/vmware-tanzu/velero).
It is a wrapper around the [prometheus-exporter](./prometheus-exporter.md) 
monitor that provides a restricted but expandable set of metrics.

<!--- SETUP --->
### Velero configuration

The Helm chart automatically enable prometheus metrics
for Velero (see [this PR](https://github.com/helm/charts/pull/19595/files))
So there is nothing to do.

### Agent configuration

This is recommended to use service discovery:

```yaml
monitors:
- type: prometheus/velero
  discoveryRule: container_image =~ "velero" && port == 8085
  port: 8085
```


## Configuration

To activate this monitor in the Smart Agent, add the following to your
agent config:

```
monitors:  # All monitor config goes under this key
 - type: prometheus/velero
   ...  # Additional config
```

**For a list of monitor options that are common to all monitors, see [Common
Configuration](../monitor-config.md#common-configuration).**


| Config option | Required | Type | Description |
| --- | --- | --- | --- |
| `httpTimeout` | no | `int64` | HTTP timeout duration for both read and writes. This should be a duration string that is accepted by https://golang.org/pkg/time/#ParseDuration (**default:** `10s`) |
| `username` | no | `string` | Basic Auth username to use on each request, if any. |
| `password` | no | `string` | Basic Auth password to use on each request, if any. |
| `useHTTPS` | no | `bool` | If true, the agent will connect to the server using HTTPS instead of plain HTTP. (**default:** `false`) |
| `httpHeaders` | no | `map of strings` | A map of HTTP header names to values. Comma separated multiple values for the same message-header is supported. |
| `skipVerify` | no | `bool` | If useHTTPS is true and this option is also true, the exporter's TLS cert will not be verified. (**default:** `false`) |
| `sniServerName` | no | `string` | If useHTTPS is true and skipVerify is true, the sniServerName is used to verify the hostname on the returned certificates. It is also included in the client's handshake to support virtual hosting unless it is an IP address. |
| `caCertPath` | no | `string` | Path to the CA cert that has signed the TLS cert, unnecessary if `skipVerify` is set to false. |
| `clientCertPath` | no | `string` | Path to the client TLS cert to use for TLS required connections |
| `clientKeyPath` | no | `string` | Path to the client TLS key to use for TLS required connections |
| `host` | **yes** | `string` | Host of the exporter |
| `port` | **yes** | `integer` | Port of the exporter |
| `useServiceAccount` | no | `bool` | Use pod service account to authenticate. (**default:** `false`) |
| `metricPath` | no | `string` | Path to the metrics endpoint on the exporter server, usually `/metrics` (the default). (**default:** `/metrics`) |
| `sendAllMetrics` | no | `bool` | Send all the metrics that come out of the Prometheus exporter without any filtering.  This option has no effect when using the prometheus exporter monitor directly since there is no built-in filtering, only when embedding it in other monitors. (**default:** `false`) |


## Metrics

These are the metrics available for this monitor.
Metrics that are categorized as
[container/host](https://docs.signalfx.com/en/latest/admin-guide/usage.html#about-custom-bundled-and-high-resolution-metrics)
(*default*) are ***in bold and italics*** in the list below.


 - ***`velero_backup_deletion_failure_total`*** (*cumulative*)<br>    Total number of failed backup deletions.
 - ***`velero_backup_failure_total`*** (*cumulative*)<br>    Total number of failed backups.
 - ***`velero_backup_partial_failure_total`*** (*cumulative*)<br>    Total number of partially failed backups.
 - ***`velero_backup_success_total`*** (*cumulative*)<br>    Total number of successful backups.
 - ***`velero_volume_snapshot_failure_total`*** (*cumulative*)<br>    Total number of failed volume snapshots.

### Non-default metrics (version 4.7.0+)

To emit metrics that are not _default_, you can add those metrics in the
generic monitor-level `extraMetrics` config option.  Metrics that are derived
from specific configuration options that do not appear in the above list of
metrics do not need to be added to `extraMetrics`.

To see a list of metrics that will be emitted you can run `agent-status
monitors` after configuring this monitor in a running agent instance.




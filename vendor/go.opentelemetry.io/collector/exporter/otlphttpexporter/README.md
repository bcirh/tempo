# OTLP/HTTP Exporter

<!-- status autogenerated section -->
| Status        |           |
| ------------- |-----------|
| Stability     | [development]: profiles   |
|               | [beta]: logs   |
|               | [stable]: traces, metrics   |
| Distributions | [core], [contrib], [k8s], [otlp] |
| Issues        | [![Open issues](https://img.shields.io/github/issues-search/open-telemetry/opentelemetry-collector?query=is%3Aissue%20is%3Aopen%20label%3Aexporter%2Fotlphttp%20&label=open&color=orange&logo=opentelemetry)](https://github.com/open-telemetry/opentelemetry-collector/issues?q=is%3Aopen+is%3Aissue+label%3Aexporter%2Fotlphttp) [![Closed issues](https://img.shields.io/github/issues-search/open-telemetry/opentelemetry-collector?query=is%3Aissue%20is%3Aclosed%20label%3Aexporter%2Fotlphttp%20&label=closed&color=blue&logo=opentelemetry)](https://github.com/open-telemetry/opentelemetry-collector/issues?q=is%3Aclosed+is%3Aissue+label%3Aexporter%2Fotlphttp) |

[development]: https://github.com/open-telemetry/opentelemetry-collector/blob/main/docs/component-stability.md#development
[beta]: https://github.com/open-telemetry/opentelemetry-collector/blob/main/docs/component-stability.md#beta
[stable]: https://github.com/open-telemetry/opentelemetry-collector/blob/main/docs/component-stability.md#stable
[core]: https://github.com/open-telemetry/opentelemetry-collector-releases/tree/main/distributions/otelcol
[contrib]: https://github.com/open-telemetry/opentelemetry-collector-releases/tree/main/distributions/otelcol-contrib
[k8s]: https://github.com/open-telemetry/opentelemetry-collector-releases/tree/main/distributions/otelcol-k8s
[otlp]: https://github.com/open-telemetry/opentelemetry-collector-releases/tree/main/distributions/otelcol-otlp
<!-- end autogenerated section -->

Export traces and/or metrics via HTTP using [OTLP](
https://github.com/open-telemetry/opentelemetry-proto/blob/main/docs/specification.md)
format.

The following settings are required:

- `endpoint` (no default): The target base URL to send data to (e.g.: https://example.com:4318).
  To send each signal a corresponding path will be added to this base URL, i.e. for traces
  "/v1/traces" will appended, for metrics "/v1/metrics" will be appended, for logs
  "/v1/logs" will be appended. 

The following settings can be optionally configured:

- `traces_endpoint` (no default): The target URL to send trace data to (e.g.: https://example.com:4318/v1/traces).
   If this setting is present the `endpoint` setting is ignored for traces.
- `metrics_endpoint` (no default): The target URL to send metric data to (e.g.: https://example.com:4318/v1/metrics).
   If this setting is present the `endpoint` setting is ignored for metrics.
- `logs_endpoint` (no default): The target URL to send log data to (e.g.: https://example.com:4318/v1/logs).
   If this setting is present the `endpoint` setting is ignored logs.
- `tls`: see [TLS Configuration Settings](../../config/configtls/README.md) for the full set of available options.
- `timeout` (default = 30s): HTTP request time limit. For details see https://golang.org/pkg/net/http/#Client
- `read_buffer_size` (default = 0): ReadBufferSize for HTTP client.
- `write_buffer_size` (default = 512 * 1024): WriteBufferSize for HTTP client.
- `encoding` (default = proto): The encoding to use for the messages (valid options: `proto`, `json`)

Example:

```yaml
exporters:
  otlphttp:
    endpoint: https://example.com:4318
```

By default `gzip` compression is enabled. See [compression comparison](../../config/configgrpc/README.md#compression-comparison) for details benchmark information. To disable, configure as follows:

```yaml
exporters:
  otlphttp:
    ...
    compression: none
```

By default `proto` encoding is used, to change the content encoding of the message configure it as follows:

```yaml
exporters:
  otlphttp:
    ...
    encoding: json
```

The full list of settings exposed for this exporter are documented [here](./config.go)
with detailed sample configurations [here](./testdata/config.yaml).
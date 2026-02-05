# Aspire Dashboard (Helm chart)

Overview
--------
The Aspire Dashboard is a lightweight OpenTelemetry dashboard (dotnet) aimed at local and small home-server use. It provides views for logs, metrics and traces with a low resource footprint compared to a full Prometheus + Grafana stack.

This chart packages the standalone Aspire Dashboard and an optional OTLP ingestion endpoint so you can deploy both the UI and a minimal OTLP receiver in-cluster.

Quick install
-------------
Add the Helm repo and install the chart:

```sh
helm repo add aspire-dashboard https://kube-the-home.github.io/aspire-dashboard-helm/
helm repo update
helm install my-aspire-dashboard aspire-dashboard/aspire-dashboard
```

Configuration highlights
------------------------
- `ui.ingress`: Enable and configure ingress host/tls for the dashboard UI.
- `otlp.clusterip.enabled`: Enable an in-cluster OTLP receiver for ingesting traces/metrics.
- `env`: Use environment variables to configure authentication and API keys.

Example values (minimal)

```yaml
ui:
  ingress:
    enabled: true
    host: aspire.example.local
    tls:
      - hosts: [aspire.example.local]
        secretName: aspire-tls

otlp:
  clusterip:
    enabled: true

env:
  - name: DASHBOARD__OTLP__AUTHMODE
    value: "ApiKey"
  - name: DASHBOARD__OTLP__PRIMARYAPIKEY
    value: "aspire"
```

Authentication
--------------
The standalone Aspire Dashboard's built-in authentication is minimal and geared toward ingestion auth (API key). For public or multi-user deployments, front the dashboard with an ingress authentication layer (OIDC, OAuth proxy) or use network-restricted access.

Troubleshooting
---------------
- Check pod logs with `kubectl -n <ns> logs deploy/my-aspire-dashboard` for auth tokens and startup issues.
- If the UI returns 404 or assets are missing, verify ingress host, service port, and TLS secrets in your `values.yaml`.

Links
-----
- Chart repository: https://github.com/kube-the-home/aspire-dashboard-helm
- Aspire documentation: https://learn.microsoft.com/en-us/dotnet/aspire/
[![Artifact Hub](https://img.shields.io/endpoint?url=https://artifacthub.io/badge/repository/aspire-dashboard)](https://artifacthub.io/packages/search?repo=aspire-dashboard&logoColor=white&color=0366D6)
![GitHub Release](https://img.shields.io/github/v/release/kube-the-home/aspire-dashboard-helm?logoColor=white&color=0366D6)


The [Dotnet Aspire Dashboard][1] is a leightweight [Open Telemetry][2] Dashboard that has mainly been developed to aid in local development.
It can help displaying Logs, Metrics and Traces.

As it is fairly leightweight it a great tool to use in small Homeserver setups, as it has a way smaller footprint than the [Kube-Prometheus-Stack][3].

If you want to deploy the Aspire Dashboard to your Kubernetes Cluster, you can use the following [Helm Chart][4].

Installing the Helm Chart.
```sh
# Adding the Repository
helm repo add aspire-dashboard https://kube-the-home.github.io/aspire-dashboard-helm/

# Installing the Chart
helm install my-aspire-dashboard aspire-dashboard/aspire-dashboard
```

## Authentication
As the Dashboard was not developed for this usecase the authentication options are a bit inconvenient.

To log-in to the Dashboard you need an authentication token, which at the moment can only be found in the application logs.

## Example Values
```yaml
ui:
    ingress:
        enabled: true
        host: aspire.example.com
        annotations: {}
        tls:
            - hosts:
                - aspire.example.com
              secretName: aspire-tls
env:
    # Setting ApiKey to be the main method of authentication
    # for ingestion
    - name: DASHBOARD__OTLP__AUTHMODE
      value: "ApiKey"
    # Setting the API Key otel applications would need to set
    # to ingest metrics
    - name: DASHBOARD__OTLP__PRIMARYAPIKEY
      value: "aspire"
```


[1]: https://learn.microsoft.com/en-us/dotnet/aspire/fundamentals/dashboard/overview?tabs=bash
[2]: https://opentelemetry.io/docs/
[3]: https://github.com/prometheus-community/helm-charts/tree/main/charts/kube-prometheus-stack
[4]: https://github.com/kube-the-home/aspire-dashboard-helm

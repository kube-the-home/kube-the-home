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

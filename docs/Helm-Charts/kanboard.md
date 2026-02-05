# Kanboard (Helm chart)

Overview
--------
This chart deploys Kanboard, a simple and lightweight project management web application (kanban-style). The chart aims to provide an easy path to run Kanboard in small clusters with minimal operational overhead.

Quick install
-------------
```sh
helm repo add kanboard https://kube-the-home.github.io/kanboard-helm/
helm repo update
helm install my-kanboard kanboard/kanboard
```

Configuration highlights
------------------------
- `ingress`: Enable ingress and configure host/tls for public access.
- `persistence`: (Planned) Currently persistence support is limited; be careful when using ephemeral storage.
- `database`: The chart currently defaults to the built-in SQLite; external DB support is limited in this version.

Example values
--------------
```yaml
ingress:
  enabled: true
  host: kanboard.example.local
  tls:
    - hosts: [kanboard.example.local]
      secretName: kanboard-tls
```

ArgoCD example
---------------
You can deploy the chart via ArgoCD by pointing the application to the chart repo and setting `helm.valuesObject` accordingly (see repository examples).

Notes & limitations
-------------------
- Persistence: the current chart release may not provide robust persistence options. For production use, add a PVC-backed storage and test backups.
- External databases: if you need PostgreSQL/MySQL, either extend the chart or run an external database and point Kanboard at it.

Troubleshooting
---------------
- Pod crashloops: inspect logs `kubectl -n <ns> logs deploy/my-kanboard` and ensure required file permissions and config are present.
- Ingress issues: ensure host and TLS secret are correctly configured.

Links
-----
- Chart repository: https://github.com/kube-the-home/kanboard-helm
- Kanboard upstream: https://github.com/kanboard/kanboard
[![Artifact Hub](https://img.shields.io/endpoint?url=https://artifacthub.io/badge/repository/kanboard)](https://artifacthub.io/packages/search?repo=kanboard)
![GitHub Release](https://img.shields.io/github/v/release/kube-the-home/kanboard-helm?logoColor=white&color=0366D6)

This Chart can be used to deploy [Kanboard](https://github.com/kanboard/kanboard).

Installing the Helm Chart.
```sh
# Adding the Repository
helm repo add kanboard https://kube-the-home.github.io/kanboard-helm/

# Installing the Chart
helm install my-kanboard kanboard/kanboard
```

### Current restrictions
- Persistence is not setup yet
- External Databases cannot be used at the moment


# Examples

A basic example for your values.yaml.

```yaml
ingress:
    enabled: true
    host: example.com
```

A basic example for your values.yaml using tls.
```yaml
ingress:
    enabled: true
    host: exmple.com
    tls:
    - hosts:
        - example.com
        secretName: kanboard-tls
```


A basic example to using this Chart with ArgoCD.

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: kanboard
  namespace: argocd
  finalizers:
  - resources-finalizers.argocd.argoproj.io
spec:
  project: default
  revisionHistoryLimit: 1
  destination:
    server: https://kubernetes.default.svc
    namespace: kanboard
  syncPolicy:
    syncOptions:
      - CreateNamespace=true
    automated:
      prune: true
      allowEmpty: true
      selfHeal: true
  source:
    repoURL: https://kube-the-home.github.io/kanboard-helm/
    targetRevision: 0.1.2
    chart: kanboard
    helm:
      valuesObject:
        ingress:
          enabled: true
          host: example.com
```

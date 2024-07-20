# Kanboard

[![Artifact Hub](https://img.shields.io/endpoint?url=https://artifacthub.io/badge/repository/kanboard)](https://artifacthub.io/packages/search?repo=kanboard)

This Chart can be used to deploy [Kanboard](https://github.com/kanboard/kanboard).

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

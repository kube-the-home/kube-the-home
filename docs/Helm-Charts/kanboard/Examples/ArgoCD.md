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

[![Artifact Hub](https://img.shields.io/endpoint?url=https://artifacthub.io/badge/repository/skooner)](https://artifacthub.io/packages/search?repo=skooner)
![GitHub Release](https://img.shields.io/github/v/release/kube-the-home/skooner-helm?logoColor=white&color=0366D6)


[Skooner][1] is a Kubernetes Dashboard that aims to help you understand and manage your cluster.
If you want to take a look at it's source code, it can be found on [Github][2].

The source code for the helm chart to deploy skooner can be found on [Github][3] as well.

Installing Skooner with the Helm Chart.

```sh
# Adding the Repository
helm repo add skooner https://kube-the-home.github.io/skooner-helm/

# Installing the Chart
helm install my-skooner skooner/skooner
```

## Authentication
If you do not specify any external authentication you need to get the access token from the respective Kubernetes Secret.

```sh
kubectl describe secret skooner-secret -n <your_skooner_namespace>
```

##### Hint
Setting up external authentication is not yet possible in the current helm chart version.

## Example Values
```yaml
ingress:
    enabled: true
    annotations:
        cert-manager.io/cluster-issuer: letsencrypt-dns01-issuer
    host: skooner.example.com
    tls:
    - hosts:
        - skooner.example.com
      secretName: skooner-tls
resources:
    requests:
        memory: "100Mi"
        cpu: "50m"
    limits:
        memory: "150Mi"
        cpu: "100m"
```

[1]: https://skooner.io/
[2]: https://github.com/skooner-k8s/skooner
[3]: https://github.com/kube-the-home/skooner-helm

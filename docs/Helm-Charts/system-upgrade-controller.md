# System Upgrade Controller (Helm chart)

Overview
--------
The System Upgrade Controller automates safe upgrades of nodes and system components in Kubernetes clusters. This chart packages the controller and related CRDs so you can define upgrade plans and let the controller orchestrate rolling updates.

When to use
-----------
- Automate cluster OS or system component upgrades on a schedule or triggered plan.
- Reduce manual intervention for rolling upgrades across node pools.

Quick install
-------------
Add the Helm repo and install the chart:

```sh
helm repo add system-upgrade-controller https://kube-the-home.github.io/system-upgrade-controller-helm/
helm repo update
helm install my-upgrade-controller system-upgrade-controller/system-upgrade-controller
```

Configuration highlights
------------------------
- `values.yaml` contains options for image, RBAC, and controller behavior.
- You can enable/disable automatic CRD installation depending on your cluster policy.

Example: create an UpgradePlan
------------------------------
Below is an abbreviated example of an UpgradePlan resource (see upstream docs for full fields):

```yaml
apiVersion: quay.io/v1
kind: UpgradePlan
metadata:
  name: upgrade-plan-example
spec:
  concurrency: 1
  nodeSelector:
    matchLabels:
      node-role.kubernetes.io/worker: ""
  upgrade: 
    image: "my-os-image:v1.2.3"
```

Upgrading and rollback
----------------------
- Use `helm upgrade` to change controller versions.
- Define conservative `concurrency` and `maxUnavailable` settings for safe rollouts.

Troubleshooting
---------------
- Check controller logs: `kubectl -n <ns> logs deploy/system-upgrade-controller`.
- Inspect UpgradePlan CR status and related events (`kubectl describe upgradeplan <name>`).

Links
-----
- Project: https://github.com/kube-the-home/system-upgrade-controller-helm
- Upstream system-upgrade-controller: https://github.com/rancher/system-upgrade-controller

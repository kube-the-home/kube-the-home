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

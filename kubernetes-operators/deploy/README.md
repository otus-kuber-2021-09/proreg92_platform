# Kubernetes version of minikube:

```sh
minikube start -p aged --kubernetes-version=v1.21.0
```
### Getting DB password from Secrets:

```yaml
  password:
    envFrom:
    - secretRef:
        name: cr-secrets
```

### Added/Changed schema validation types for using Secrets:

```yaml
    password:
      type: object
    secretName:
      type: string
    secretRef:
      type: object
    envFrom:
      type: object
```

### Added required fields/properties for schema validation of CRD:

```yaml
    required:
      - image
      - database
      - storage_size
```
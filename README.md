# futu-opend

## 1. Create RSA key

```bash
openssl genrsa -out futu.pem 1024
```

# Install

myValue.yaml

```yaml
secrets:
  futu-account-id: "123456"
  futu-account-pwd: 123456
futuopend:
  rsaFile: futu.pem # futu.pem is the path of the RSA key
```

```bash
helm upgrade --install  futu-opend . -f myValue.yaml
```

# Test

```bash
nc -vz 127.0.0.1 11111
```

# Attach Futu OpenD

```bash
export POD_NAME=$(kubectl get pods --namespace {{ .Release.Namespace }} -l "app.kubernetes.io/name={{ include "futu-opend.name" . }},app.kubernetes.io/instance={{ .Release.Name }}" -o jsonpath="{.items[0].metadata.name}")
export CONTAINER_NAME=$(kubectl get pod --namespace {{ .Release.Namespace }} $POD_NAME -o jsonpath="{.spec.containers[0].name}")
kubectl attach $POD_NAME -c $CONTAINER_NAME -i -t
```

# Port Forward

```bash
kubectl --namespace default port-forward svc/futu-opend 11111:11111
```
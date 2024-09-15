# futu-opend


## Usage

[Helm](https://helm.sh) must be installed to use the charts.  Please refer to
Helm's [documentation](https://helm.sh/docs) to get started.

Once Helm has been set up correctly, add the repo as follows:

```bash
helm repo add ib-gateway https://manhinhang.github.io/futu-opend-helm-chart/
```

If you had already added this repo earlier, run `helm repo update` to retrieve
the latest versions of the packages.  You can then run `helm search repo
ib-gateway` to see the charts.


## 1. Create RSA key

```bash
openssl genrsa -out futu.pem 1024
```

# Install

## Use custom values.yaml

myValue.yaml

```yaml
secrets:
  futu-account-id: "123456"
  futu-account-pwd: 123456
futuOpenD:
  rsaFile: rsa string # futu.pem is content of the RSA key
```

```bash
helm upgrade --install  futu-opend futu-opend/futu-opend -f myValue.yaml
```

## Use inline config

```bash
helm upgrade --install  futu-opend futu-opend/futu-opend --set secrets.futu-account-id=123456 --set secrets.futu-account-pwd=123456 --set futuopend.rsaFile=futu.pem
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

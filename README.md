# chart-ingress-controllers
This repository contains 2 charts that are used to deploy internal and external ingress to kubernetes.
- external-ingress
- internal-ingress

## Installing
First install `default-backend` chart
```
helm install --name default-backend chartmuseum/default-backend --namespace external-ingress
```

After that, install `external-ingress` chart
```
helm install --name external-ingress chartmuseum/external-ingress --namespace external-ingress
```
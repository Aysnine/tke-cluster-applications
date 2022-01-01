# tke-cluster-applications

Need Helm 3+.

## Traefik - 2.5

presets:

- Exposed host 80 and 443 ports.
- Service use ClusterIP (Avoid expensive default LB).
- Stable DaemonSet traefik pod.

```sh
# helm repo add traefik https://helm.traefik.io/traefik
# helm pull traefik/traefik --untar

helm install traefik ./traefik # install

helm upgrade traefik ./traefik # upgrade
helm upgrade traefik ./traefik --install # if install before upgrade

helm uninstall traefik # uninstall
```

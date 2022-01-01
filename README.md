# tke-cluster-applications

## traefik

```sh
# helm repo add traefik https://helm.traefik.io/traefik
# helm pull traefik/traefik --untar

helm install traefik ./traefik # install

helm upgrade traefik ./traefik # upgrade
helm upgrade traefik ./traefik --install # if install before upgrade

helm uninstall traefik # uninstall
```

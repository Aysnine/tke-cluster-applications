# tke-cluster-applications

Need Helm 3+.

**Be care for comments that starts with `// !` in `values.yaml`**

## Traefik - 2.5

Requirements:

- A default PersistentVolume >= 10G

Changes:

- Exposed host 80 and 443 ports.
- Service use ClusterIP (Avoid expensive default LB).
- TLS with persistence.

```sh
# helm repo add traefik https://helm.traefik.io/traefik
# helm pull traefik/traefik --untar

helm install traefik ./traefik # install

helm upgrade traefik ./traefik # upgrade
helm upgrade traefik ./traefik --install # if install before upgrade

helm uninstall traefik # uninstall
```

References: 

- https://www.qikqiak.com/post/automatic-https-with-traefik2/

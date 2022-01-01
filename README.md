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

Quick started:

```sh
# Please clone this repo to local first

helm upgrade traefik ./traefik --install # if install before upgrade

helm uninstall traefik # uninstall

# ! Change `certificatesResolvers.default.acme.caserver` when production
```

References: 

- https://www.qikqiak.com/post/automatic-https-with-traefik2/

### Routing usage: Expose your service by IngressRoute

Expose web service `my-nginx-app:80` to `https://my-nginx-app.com` with auto cert and https-redirect.

``` yaml
# expose service to http://my-nginx-app.com
apiVersion: traefik.containo.us/v1alpha1
kind: IngressRoute
metadata:
  name: my-nginx-app
spec:
  entryPoints:
    - web
  routes:
    - match: Host(`my-nginx-app.com`)
      kind: Rule
      # a service reference
      services:
        - name: my-nginx-app
          port: 80
      # redirect to https by custom middleware
      middlewares:
        - name: my-nginx-app-https-redirect-middleware
---
# custom middleware for redirect to https
apiVersion: traefik.containo.us/v1alpha1
kind: Middleware
metadata:
  name: my-nginx-app-https-redirect-middleware
spec:
  redirectScheme:
    scheme: https
    permanent: true
---
# expose service to https://my-nginx-app.com
apiVersion: traefik.containo.us/v1alpha1
kind: IngressRoute
metadata:
  name: my-nginx-app-tls
spec:
  entryPoints:
    - websecure
  routes:
    - match: Host(`my-nginx-app.com`)
      kind: Rule
      # a service reference
      services:
        - name: my-nginx-app
          port: 80
  tls:
    certResolver: default # Use default. Auto cert from Let's Encrypt
```

# ArgoCD

## Commands
### Retrieve initial password
```bash
argocd admin initial-password
```


## Apps
### Helm (with separate values.yml)
```yaml
# The path must be part of the helm specification
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: cert-manager
  namespace: argocd
  annotations:
    argocd.argoproj.io/sync-wave: "-2"
spec:
  project: default
  sources:
  - repoURL: https://charts.jetstack.io
    chart: cert-manager
    targetRevision: v1.20.1
    helm:
      valueFiles:
      - $values/2-App-Setup/argocd/apps/values/cert-manager.yml
  - repoURL: https://github.com/thabich/kubernetes-dev-environment.git
    targetRevision: feature/setup
    ref: values

  destination:
    server: https://kubernetes.default.svc
    namespace: cert-manager
  syncPolicy:
    automated:
      selfHeal: true
      prune: true
    syncOptions:
      - ServerSideApply=true
      - CreateNamespace=true



### Kustomize
```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: gateway-api-crds
  namespace: argocd
  labels:
    app.kubernetes.io/part-of: bootstrap
  annotations:
    argocd.argoproj.io/sync-wave: "-1"

spec:
  project: default
  source:
    repoURL: https://github.com/kubernetes-sigs/gateway-api.git
    targetRevision: v1.4.1
    path: config/crd
  destination:
    server: https://kubernetes.default.svc
    namespace: kube-system
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
```

### Git Only (recursive)
```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: app-of-apps
  namespace: argocd
  annotations:
    argocd.argoproj.io/sync-wave: "-1"
spec:
  project: default
  source:
    repoURL: https://github.com/thabich/kubernetes-dev-environment.git
    targetRevision: feature/setup
    path: 2-App-Setup/argocd/apps/enabled
    directory:
      recurse: true
  destination:
    server: https://kubernetes.default.svc
    namespace: argocd
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
```

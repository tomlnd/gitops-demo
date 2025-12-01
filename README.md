# GitOps Demo

A demo GitOps repo that uses ArgoCD to deploy a bunch of example apps and configurations to a Kubernetes cluster.

## Structure

`./cluster-bootstrap.yaml` bootstraps ArgoCD's resources into the cluster.

`./argocd` directory contains everything related to ArgoCD itself, such as its own Helm values to track itself, and ApplicationSets to deploy everything into a cluster.

`./apps` directory contains 3 directories which define the ArgoCD project the apps in them are deployed to. `./apps/infrastructure` holds every core-software in the cluster, while `./apps/system` holds system/cluster-wide configurations, such as CoreDNS configs and StorageClasses.

`./apps/production` holds a simple Helm chart which is deployed to each directory under `./tenants`.  
That one is controlled via `./argocd/appset-tenant-production-charts.yaml` which uses a hacky workaround (see my comment at [https://github.com/argoproj/argo-cd/issues/20604#issuecomment-2485520650](https://github.com/argoproj/argo-cd/issues/20604#issuecomment-2485520650)) to allow overriding a chart's values in ArgoCD when the value overrides are outside the chart's directory.

```
├── apps
│   ├── infrastructure
│   │   ├── argocd-image-updater
│   │   ├── cert-manager
│   │   ├── cnpg-operator
│   │   ├── ingress-nginx-internal
│   │   ├── ingress-nginx-public
│   │   ├── k8ssandra-operator
│   │   ├── keycloak-operator
│   │   ├── kube-prometheus-stack
│   │   ├── openebs
│   │   ├── reloader
│   │   ├── sealed-secrets
│   │   └── trust-manager
│   ├── production
│   │   └── hello-app
│   └── system
│       ├── calico
│       ├── coredns
│       └── storageclass
├── argocd
│   ├── appset-apps.yaml
│   ├── appset-tenant-manifests.yaml
│   ├── appset-tenant-production-charts.yaml
│   ├── argocd-helm-values.yaml
│   ├── argocd-oidc-secret.yaml
│   ├── argocd-projects.yaml
│   └── argocd.yaml
├── cluster-bootstrap.yaml
├── README.md
└── tenants
    ├── demo1
    │   └── values.yaml
    └── demo2
        └── values.yaml
```

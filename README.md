# Platform GitOps

Flux GitOps repo for our Kubernetes platform. The cluster syncs from this repo automatically.

Platform overview and bootstrap instructions are in [platform-infra](https://github.com/grupa-tri/platform-infra).

## Status

Cluster path is `clusters/my-cluster`. Flux reconciles infrastructure components and tenant instances from here.

## Layout

```
clusters/my-cluster/
  flux-system/          Flux controllers and sync config
  infrastructure/       Platform stack and Crossplane definitions
    tenants/            One ApplicationInstance YAML per tenant
```

## Platform components

| Component | Purpose |
|-----------|---------|
| ExternalDNS | Creates DNS records for tenant ingresses |
| cert-manager | Issues TLS certs via DNS-01 (Cloud DNS) |
| External Secrets | Pulls secrets from GCP Secret Manager |
| CloudNativePG | Manages Postgres clusters per tenant |
| Crossplane | Provisions tenant stacks from `ApplicationInstance` CRs |

Crossplane definitions live in `infrastructure/crossplane-infra/` (XRD, Composition, EnvironmentConfig).

## How to add a new tenant

1. Copy `clusters/my-cluster/infrastructure/tenants/tenant-a.yaml`.
2. Set a unique `metadata.name`, `spec.tenantId`, and `spec.host` (subdomain of `upondi.com`).
3. Pick a cert issuer. Use `letsencrypt-staging` for test tenants, `letsencrypt-prod` for production.

Follow [How to contribute](https://github.com/grupa-tri/platform-infra#how-to-contribute) in platform-infra to get the change merged.

After merge, Flux picks up the change and Crossplane creates

- tenant namespace with ResourceQuota and LimitRange
- NetworkPolicies for soft multi-tenancy
- CloudNativePG Postgres cluster
- backend and frontend Deployments
- Ingress with TLS and ExternalDNS record

Example tenant file

```yaml
apiVersion: platform.example.org/v1alpha1
kind: ApplicationInstance
metadata:
  name: tenant-c
spec:
  tenantId: tenant-c
  host: tenant-c.upondi.com
  clusterIssuer: letsencrypt-staging
```

## How to test a new app version

Add a staging tenant with `clusterIssuer: letsencrypt-staging`. Override images per tenant if needed

```yaml
spec:
  backendImage: ghcr.io/grupa-tri/app-backend:1.1.0
  frontendImage: ghcr.io/grupa-tri/app-frontend:1.1.0
```

To roll out a version to all tenants, bump the tags in `infrastructure/crossplane-infra/environmentconfig-app-images.yaml`.

## CI

Pull requests validate Kubernetes manifests with kubeconform via [.github/workflows/kubeconform.yml](.github/workflows/kubeconform.yml).

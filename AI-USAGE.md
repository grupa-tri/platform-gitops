# This is the main documentation file for documenting AI usage

## Github Actions

### Promt

> Mache mir eine github action um kuberenetes yamls zu validieren

### Validated

Checked latest kubeconform version https://github.com/yannh/kubeconform

## External-DNS

### Promt

> Create Flux external-dns setup for GKE

### Validated

Validated using https://artifacthub.io/packages/helm/external-dns/external-dns

Domain and service account set properly

## Cert-Manager

### Promt

> Create Flux cert-manager setup for GKE

### Validated

Validated using https://artifacthub.io/packages/helm/cert-manager/cert-manager

Service account set properly

## External-Secrets

### Promt

> Create Flux external-secrets setup for GKE

### Validated

Validated using https://artifacthub.io/packages/helm/external-secrets-operator/external-secrets

Service account set properly

## Cloudnative-pg

### Promt

> Create Flux cloudnative-pg setup for GKE

### Validated

Validated using https://artifacthub.io/packages/helm/cloudnative-pg/cloudnative-pg

## Crossplane

### Promt

> Create Flux crossplane setup for GKE

### Validated

Validated using https://artifacthub.io/packages/helm/crossplane/crossplane

### Promt - EnvironmentConfig

> Which possibilities does crossplane have to define variables outside of the composition for the composition?

### Validated - EnvironmentConfig

EnvironmentConfig looked like a good solution. Validated using https://docs.crossplane.io/latest/composition/environment-configs/

AI offered integration of the config without a function, but official documentation required the function.

### Promt - CompositeResourceDefinition

> Erstelle mir eine basic CompositeResourceDefinition für die App aus der Angabe. (Kontext: Angabe)

### Validated - CompositeResourceDefinition

Validated with own domain knowledge

### Promt - RBAC

> Erstelle mir die RBAC das crossplane die ressourcen aus der composition erstellen darf. (Kontext: Composition)

### Validated - RBAC

Validated through testing with composition

### Prompt - Composition

The configurations were created with AI assistance. Each team
member worked independently across multiple parallel tabs, conducting individual
AI conversations to explore different solution approaches for the respective
components (GitHub Actions, External-DNS, Cert-Manager, External-Secrets,
CloudNativePG, Crossplane) and to weigh them against one another.

The AI-generated suggestions were not adopted unchecked. Instead, they were
systematically cross-checked against the official documentation:

- **Kubernetes resources** were verified for correctness and currency against the
  official Kubernetes documentation as well as the respective Helm chart
  documentation (Artifact Hub).
- **Crossplane-specific constructs** (EnvironmentConfig, CompositeResourceDefinition,
  Compositions, RBAC) were validated against the official Crossplane documentation
  and, in some cases, verified through our own tests in the cluster.

In several cases, the AI suggestion deviated from the official recommendation
(e.g., the EnvironmentConfig integration without a function). Such deviations were
identified, documented, and corrected in favor of the documentation-compliant
variant.

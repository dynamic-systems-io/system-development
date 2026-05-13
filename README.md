# system-development

Central development system for platform developer experience services.

## Scope

- Backstage developer portal
- Backstage catalog ingestion for platform component repositories
- GitHub OAuth authentication for Backstage via External Secrets
- Istio ingress route for Backstage

## Layout

- `argocd/apps/` - Argo CD Applications for development system components
- `integrations/` - shared integration manifests (ingress, mesh wiring)
- `security/` - ExternalSecret resources + SecretStore template for development-system workloads
- `docs/` - operational runbooks

## Access endpoints (kind default)

- Backstage: `http://backstage.localhost:8080` (or `http://backstage.127.0.0.1.nip.io:8080`)

## Secret prerequisites

Backstage auth expects a namespace-scoped `SecretStore` named `secret-store` in namespace `development-system` and a remote secret at key `development-system/backstage/github-oauth` with properties:

- `clientId`
- `clientSecret`

See:

- `security/secret-store.example.yaml`
- `docs/backstage-oauth-runbook.md`

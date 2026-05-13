# system-development

Central development system for platform developer experience services.

## Scope

- Developer portal (Backstage-based)
- Developer portal catalog ingestion for platform component repositories
- Keycloak OIDC authentication for the developer portal via External Secrets
- Istio ingress route for the developer portal

## Layout

- `argocd/apps/` - Argo CD Applications for development system components
- `integrations/` - shared integration manifests (ingress, mesh wiring, SSO auth policy examples)
- `security/` - Crossplane OIDC + SecretStore provider claims + ExternalSecret resources for development-system workloads
- `docs/` - operational runbooks

## Access endpoints (kind default)

- Developer portal: `http://backstage.localhost:8080` (or `http://backstage.127.0.0.1.nip.io:8080`)

## Secret prerequisites

By default this repo provisions local Keycloak OIDC (`security/00-oidc-provider-local-keycloak.yaml`) and `secret-store` via Crossplane using OpenBao in-cluster (`security/00-openbao-secret-store-provider.yaml`).

Developer portal access is enforced centrally at the Istio gateway via oauth2-proxy. App-level login is disabled for a unified login flow.

Cloud options are always supported with provider claim examples in the same folder:
- AWS Secrets Manager: `security/aws-secret-store-provider.example.yaml`
- Azure Key Vault: `security/azure-secret-store-provider.example.yaml`

See:

- `security/secret-store.example.yaml`
- `docs/developer-portal-oauth-runbook.md`

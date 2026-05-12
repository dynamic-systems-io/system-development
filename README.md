# system-development

Central development system for platform developer experience services.

## Scope

- Backstage developer portal
- Backstage catalog ingestion for platform component repositories
- Istio ingress route for Backstage

## Layout

- `argocd/apps/` - Argo CD Applications for development system components
- `integrations/` - shared integration manifests (ingress, mesh wiring)

## Access endpoints (kind default)

- Backstage: `http://backstage.localhost:8080` (or `http://backstage.127.0.0.1.nip.io:8080`)

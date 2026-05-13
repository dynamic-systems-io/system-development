# Platform UI SSO Runbook (development-system)

## 0) Local OIDC provider (Keycloak)

This repo provisions local Keycloak via Crossplane claim:

- `system-development/security/00-oidc-provider-local-keycloak.yaml`
- exposed at: `http://keycloak.localhost:8080`

Bootstrap realm + OIDC clients + demo user:

```bash
./infrastructure/scripts/bootstrap-keycloak-kind-sso.sh
```

## 1) Provision namespace-scoped SecretStores

Preferred (Crossplane + local OpenBao default):

```bash
kubectl apply -f system-development/security/00-openbao-secret-store-provider.yaml
```

and for Istio SSO components (from observability integrations):

```bash
kubectl apply -f system-observability/integrations/00-openbao-secret-store-provider-istio.yaml
```

Managed backends (examples):

- `system-development/security/aws-secret-store-provider.example.yaml`
- `system-development/security/azure-secret-store-provider.example.yaml`

## 2) Seed local OpenBao secrets

For local kind + OpenBao:

```bash
./infrastructure/scripts/seed-openbao-kind-secrets.sh
```

This seeds:

- `observability-system/grafana/admin`
- `istio-system/keycloak/oauth2-proxy`

## 3) Verify SSO resources

```bash
kubectl -n istio-system get authorizationpolicy require-sso-at-ingressgateway
kubectl -n istio-system get secret oauth2-proxy-oidc-auth
kubectl -n observability-system get secret observability-system-grafana-admin
kubectl -n istio-system get deploy oauth2-proxy
```

Optional automated smoke test:

```bash
STRICT_GATEWAY_SSO=true ./infrastructure/scripts/smoke-ui-sso.sh
```

## 4) Login

Open any UI:

- `http://backstage.localhost:8080`
- `http://grafana.localhost:8080`
- `http://kiali.localhost:8080`
- `http://argocd.localhost:8080`

You should be redirected to centralized login through:

- `http://auth.localhost:8080`

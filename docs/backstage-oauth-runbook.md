# Backstage OAuth Runbook (development-system)

## 1) Create GitHub OAuth App

- GitHub settings → Developer settings → OAuth Apps → New OAuth App
- Application URL: `http://backstage.localhost:8080`
- Callback URL: `http://backstage.localhost:8080/api/auth/github/handler/frame`

Save:
- Client ID
- Client Secret

## 2) Create namespace-scoped SecretStore

Use `security/secret-store.example.yaml` as template.

- name: `secret-store`
- namespace: `development-system`

Apply:

```bash
kubectl apply -f system-development/security/secret-store.yaml
```

## 3) Add remote secret values

`ExternalSecret` expects remote key:

- key: `development-system/backstage/github-oauth`
- properties:
  - `clientId`
  - `clientSecret`

Update your secret manager accordingly.

## 4) Verify ExternalSecret reconciliation

```bash
kubectl -n development-system get externalsecret development-system-backstage-auth
kubectl -n development-system get secret development-system-backstage-auth
```

## 5) Verify Backstage auth endpoint

```bash
curl -I 'http://backstage.localhost:8080/api/auth/github/start?env=development'
```

Expected: `302 Found`.

## 6) Login

Open:

- `http://backstage.localhost:8080`

Choose GitHub sign-in.

# Configuration Contract

This repository stores external configuration for Esoft 7Card services.

## Naming

Use this naming pattern:

```text
application.yml
application-{environment}.yml
```

Supported environments:

- `local`
- `dev`
- `prod`

## Precedence

Consumers should apply configuration in this order, with later files overriding
earlier values:

1. `application.yml`
2. `application-{environment}.yml`
3. runtime environment variables or secret manager values

## Secrets

Never commit secret values. Store secret references only:

```yaml
spring:
  datasource:
    username: ${PROD_DB_USERNAME}
    password: ${PROD_DB_PASSWORD}
```

Allowed in this repository:

- host names that are safe to disclose
- feature flag defaults
- pool sizes and timeouts
- log levels
- non-sensitive service URLs

Not allowed in this repository:

- passwords
- API keys
- JWT signing keys
- private certificates
- production customer data

## Change Process

- Use pull requests for shared and production changes.
- Explain the service, environment, and expected behavior change.
- Roll out production changes through the deployment pipeline after approval.
- Revert by restoring the previous committed value and redeploying/reloading the
  affected service.

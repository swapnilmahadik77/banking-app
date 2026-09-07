# Banking Platform Central Configuration

Git-backed configuration repository for Spring Cloud Config Server. Services load
`application.yml`, the active environment file, and their service-specific file.

## Naming convention

- Shared defaults: `application.yml`
- Shared environment overrides: `application-{profile}.yml`
- Service configuration: `{spring.application.name}.yml`
- Service/environment overrides: `{spring.application.name}-{profile}.yml`

Recommended profiles are `local`, `dev`, `test`, `staging`, and `prod`.

## Client bootstrap

Each microservice should keep only this bootstrap configuration locally:

```yaml
spring:
  application:
    name: account-service
  config:
    import: optional:configserver:${CONFIG_SERVER_URL:http://localhost:8888}
  cloud:
    config:
      fail-fast: true
      retry:
        max-attempts: 6
        initial-interval: 1000
        multiplier: 1.5
        max-interval: 5000
```

Never commit credentials, keys, tokens, connection strings, or certificates.
Production secrets should be injected by Vault/Kubernetes Secrets and referenced
with environment placeholders.


# esoft-7card-config

Centralized configuration repository for the Esoft 7Card platform.

This repository mirrors the runtime configuration from
`C:\Users\J Pc\Desktop\workspace\esoft-7card-server\src\main\resources` so the
server can consume externalized application settings from a central location.
Keep non-secret defaults here, and inject secrets through a secret manager or
runtime environment variables.

## Layout

```text
.
|-- application.yml                  # shared defaults
|-- application-local.yml            # local H2 overrides
|-- application-dev.yml              # development PostgreSQL overrides
|-- application-prod.yml             # production overrides
`-- docs/
    `-- configuration-contract.md
```

## Usage

For Spring Cloud Config-style clients, the application name is `login`. Use the
application name and profile to resolve configuration:

```text
/{application}/{profile}
```

Examples:

```text
/login/local
/login/dev
/login/prod
```

The default profile is `local`, as defined in `application.yml`.

## Rules

- Commit defaults that are safe to share with developers.
- Do not commit passwords, API keys, private certificates, tokens, or customer
  data.
- Reference secrets through placeholders such as `${DB_PASSWORD}`.
- Prefer environment-specific override files over duplicating entire configs.
- Keep production changes small, reviewed, and traceable through pull requests.

## Validation

YAML files can be checked locally with any standard YAML parser. A simple
PowerShell smoke test is:

```powershell
Get-ChildItem -Recurse -Filter *.yml | ForEach-Object {
  Write-Host "Checking $($_.FullName)"
  npx --yes js-yaml "$($_.FullName)" > $null
  if ($LASTEXITCODE -ne 0) { exit $LASTEXITCODE }
}
```

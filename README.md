# AI Job Agent Config Server repository

This repository holds shared, non-secret service configuration. Secret values are placeholders resolved from environment variables by each Spring service at startup.

## Local Auth Service environment

Set these in the same PowerShell session or IDE run configuration used to launch Auth Service. Do not commit actual values.

```powershell
$env:DB_USERNAME = "postgres"
$env:DB_PASSWORD = "your-local-postgres-password"
$bytes = New-Object byte[] 48
[System.Security.Cryptography.RandomNumberGenerator]::Fill($bytes)
$env:JWT_SECRET = [Convert]::ToBase64String($bytes)
```

The default mail settings target a local SMTP sink at `localhost:1025`. To use Gmail SMTP for development, set `MAIL_HOST=smtp.gmail.com`, `MAIL_PORT=587`, `MAIL_USERNAME`, `MAIL_PASSWORD`, `MAIL_SMTP_AUTH=true`, and `MAIL_SMTP_STARTTLS=true` in the process environment. Use a newly issued app password, never a password that has appeared in a config file or Git history.

## Important

- `auth-service.yaml` uses `ddl-auto: update` for local development. Production should use Flyway or Liquibase migrations and `ddl-auto: validate`.
- A missing `JWT_SECRET` should prevent Auth Service from starting; do not add a checked-in fallback.
- Revoke any credential that was previously committed. Replacing it in the latest file does not remove the old value from Git history.

# Flyway Action

This action runs [Flyway v9][flyway] to migrate database. It aims to migrate database for testing in workflows.

## Usage

```yaml
name: Main
on:
  - push
jobs:
  test:
    runs-on: ubuntu-latest
    services:
      postgres:
        image: postgres
        env:
          POSTGRES_DB: db
          POSTGRES_USER: user
          POSTGRES_PASSWORD: password
        options: >-
          --health-cmd pg_isready
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5
    steps:
      - uses: actions/checkout@v2
      - uses: joshuaavalon/flyway-action@v3.0.0
        with:
          url: jdbc:postgresql://postgres:5432/db
          user: user
          password: password
      - run: echo 'testing'
```

Currently, it supports `url`, `user`, `password`, `initSql`, `version` and `locations`. `locations` are default to `filesystem:./sql`. `version` points to the flyway version to be used.

Extra configurations can be passed via environment variables.

```yaml
- uses: joshuaavalon/flyway-action@v3.0.0
  with:
    url: jdbc:postgresql://postgres:5432/db
    user: user
    password: password
  env:
    FLYWAY_VALIDATE_MIGRATION_NAMING: true
```

For details, please check out Flyway [documentation].

[flyway]: https://flywaydb.org/
[documentation]: https://documentation.red-gate.com/fd/parameters-224919673.html

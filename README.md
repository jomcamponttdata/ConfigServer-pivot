# config-server

Spring Cloud Config Server (native backend) for the banking microservices. Centralizes **non-sensitive** configuration for:

- `customer-service`
- `account-service`
- `credit-service`
- `transaction-service`

Sensitive values (database passwords) are intentionally **not** stored here. They remain environment placeholders in each client's local `application.yml` and will be moved to HashiCorp Vault in Part III.

## Run

```bash
# from repository root
mvnw.cmd -f config-server/pom.xml spring-boot:run
```

Server starts on `http://localhost:8888` (override with `CONFIG_SERVER_PORT`).

## Verify

```bash
curl http://localhost:8888/customer-service/default
curl http://localhost:8888/account-service/default
curl http://localhost:8888/credit-service/default
curl http://localhost:8888/transaction-service/default
```

## Client usage

Each microservice already declares:

```yaml
spring:
  config:
    import: optional:configserver:${CONFIG_SERVER_URL:http://localhost:8888}
```

The import is `optional`, so services still start with their local fallback when the Config Server is unavailable.

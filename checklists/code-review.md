# Code review checklist

Read the code where money and data pass first. Everything else second.

## Concurrency and retries

- [ ] Every operation that costs money or creates a record another system reacts to is idempotent, with a key scoped to the principal and stored before the side effect.
- [ ] Read-modify-write sequences on shared rows use a transaction with the right isolation level or an atomic update (`UPDATE ... SET balance = balance - :amount WHERE balance >= :amount`).
- [ ] Unique constraints exist in the database for every "must be unique" rule, not only in application code.
- [ ] Retries have a maximum, a backoff and a jitter. Infinite retries are a self-inflicted DDoS.
- [ ] Background jobs are safe to run twice and safe to run late.

## Error handling

- [ ] Errors at boundaries (network, database, third party) are caught, logged with context, and turned into a decision: retry, degrade, fail.
- [ ] No empty `catch`. No `catch` that swallows and returns a default that looks like success.
- [ ] Errors shown to users do not leak internals (stack traces, SQL, file paths).
- [ ] Timeouts are set on every outbound call and are shorter than the inbound timeout.

## Input and output

- [ ] Every input is validated at the boundary with a schema; the validated type is what flows inward.
- [ ] Output encoding matches the sink: HTML escaping for HTML, parameterised queries for SQL, no string concatenation for shell commands.
- [ ] File uploads: type checked by content, size limited, stored outside the web root or in object storage, served with a fixed content type.
- [ ] Pagination on every list endpoint; no unbounded `SELECT *`.

## Secrets and configuration

- [ ] No secrets in the repository, including history. Check with a scanner.
- [ ] Secrets per environment and per purpose. Development never shares a credential with production.
- [ ] Configuration that changes behaviour is read once at startup and validated; missing required values fail fast.

## Data

- [ ] Every table with tenant data carries the tenant and every query filters by it (or row-level security enforces it).
- [ ] Personal data fields are known, minimal, and have a retention rule.
- [ ] Soft deletes, if used, are filtered everywhere and have a hard-delete job for privacy requests.
- [ ] Migrations follow expand, migrate, contract.

## Tests that matter

- [ ] A test for each money or irreversible path, including the retry and the concurrent-duplicate case.
- [ ] A test that logs in as tenant A and requests tenant B's resources through each list and detail endpoint.
- [ ] Integration tests run against a real database, not a mock.
- [ ] Assertions are not commented out. Yes, check.

## Observability

- [ ] Structured logs with a request id that crosses service boundaries.
- [ ] Metrics for rate, errors and latency (p50, p95, p99) per endpoint and per dependency.
- [ ] An alert exists for the failure mode of each critical path, with an owner.

## Dependencies

- [ ] Lockfile committed; installs are reproducible.
- [ ] Known vulnerabilities checked in CI; a policy for how fast each severity is fixed.
- [ ] No dependency doing something a few lines of code would do more safely.

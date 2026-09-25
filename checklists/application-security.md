# Application security checklist

Ordered by how often each item is the finding that matters.

## Authentication

- [ ] Passwords hashed with a slow, salted algorithm (argon2id or bcrypt with a current cost). Never reversible.
- [ ] Rate limiting and lockout on login, password reset and OTP endpoints, per account and per IP.
- [ ] Sessions: secure, HttpOnly, SameSite cookies; rotation on login; server-side revocation possible.
- [ ] Password reset tokens single-use, short-lived, bound to the account, not guessable.
- [ ] MFA available; required for admin roles.
- [ ] If an identity provider is used: the mapping from provider subject id to local user is the boundary. What happens when the mapping is wrong or the provider is down?

## Authorisation

- [ ] Every endpoint checks that the principal may act on **that** resource, not just that the principal is logged in (IDOR is the most common critical finding).
- [ ] Authorisation is enforced server-side in one place, not scattered in handlers.
- [ ] Admin functions are on separate routes with separate checks and are logged.
- [ ] Multi-tenant filters everywhere data is read: queries, caches, search, files, queues.

## Injection and encoding

- [ ] SQL through parameters, never concatenation. Same for NoSQL, LDAP, shell.
- [ ] HTML output escaped by the template engine; no `innerHTML` with user data.
- [ ] Content Security Policy that forbids inline scripts, with `frame-ancestors` set.
- [ ] Redirect targets validated against an allow-list.
- [ ] Deserialisation of untrusted input avoided or done with a safe schema.

## Data exposure

- [ ] Error responses and logs never contain secrets, tokens, passwords or full card numbers.
- [ ] API responses return only the fields the client needs; no `SELECT *` serialised to JSON.
- [ ] Personal data encrypted in transit (TLS 1.2+) and at rest; keys managed outside the application.
- [ ] Directory listing off; source maps, `.git`, `.env`, backups not served.

## Headers and transport

- [ ] HTTPS only, HSTS with a long max-age.
- [ ] `X-Content-Type-Options: nosniff`, `Referrer-Policy`, `Permissions-Policy` restricting camera, microphone, geolocation.
- [ ] Cookies scoped to the exact host and path they need.
- [ ] CORS allow-list of origins; never `*` with credentials.

## Dependencies and build

- [ ] Dependencies scanned for known vulnerabilities in CI.
- [ ] Lockfile committed; builds reproducible.
- [ ] Third-party scripts on the page inventoried; each one is a supply-chain trust decision.

## Secrets

- [ ] No secrets in code, config files in the repository, container images or client bundles.
- [ ] Rotation procedure exists for each secret and was used at least once.
- [ ] The API key for sending email is not the key for reading analytics. One purpose per credential.

## Logging and detection

- [ ] Authentication events, authorisation failures and admin actions logged with who, what, when, from where.
- [ ] Alerts for spikes in 401/403/500 and for logins from new countries on admin accounts.
- [ ] Logs cannot be modified by the application user.

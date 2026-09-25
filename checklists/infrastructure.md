# Infrastructure checklist

The question behind every item: what happens on the day this fails, and who finds out first.

## Backups

- [ ] Automated, off-site, encrypted, with retention that covers the longest time a corruption could go unnoticed (30 days minimum).
- [ ] **Restored in the last 90 days**, timed, documented. A backup that was never restored does not exist.
- [ ] Point-in-time recovery enabled for the primary database.
- [ ] Backups of object storage and of configuration, not only of the database.

## Access

- [ ] Named accounts, no shared logins. Every access traceable to a person.
- [ ] Least privilege: production access is rare, logged and time-limited.
- [ ] MFA on the cloud console, the registrar, the DNS provider, the email provider and the repository host. These five accounts own everything else.
- [ ] Offboarding checklist that revokes every access on the day someone leaves.
- [ ] SSH: keys only, no root login, non-standard port is not security but reduces noise.

## Updates

- [ ] OS and runtime patched on a schedule; critical patches within days.
- [ ] Certificates renewed automatically, including the internal ones, the API subdomain and the one pinned in the mobile app. Alert 14 days before expiry.
- [ ] Container base images rebuilt regularly, not only when the application changes.

## Network

- [ ] Only the ports that must be public are public. The database, the cache and the queue are never reachable from the internet.
- [ ] A WAF or edge proxy in front of the application, with rate limits.
- [ ] Industrial or internal networks segmented from the corporate and public ones.
- [ ] DNS records inventoried; dangling records to decommissioned services removed (subdomain takeover).

## Deploy and rollback

- [ ] Deploys are automated, reproducible and logged. No `scp` to production.
- [ ] Rollback is a command, rehearsed, that works with the current schema.
- [ ] Health checks gate traffic to new instances.
- [ ] Environment parity: staging runs the same versions and the same infrastructure shape as production.

## Observability

- [ ] Centralised logs with retention.
- [ ] Metrics and dashboards for saturation of CPU, memory, disk, connections and queue depth.
- [ ] Uptime checks from outside the network.
- [ ] Alerts route to a person, with an escalation path, not to a channel nobody reads at 3 am.

## Cost

- [ ] Monthly cost known per environment and per major component.
- [ ] Budget alerts at 80 % and 100 %.
- [ ] Orphaned resources (volumes, snapshots, IPs, idle instances) reviewed quarterly.

## Documentation

- [ ] An architecture diagram that matches reality, dated.
- [ ] Runbooks for: restart each service, restore the database, rotate each secret, handle the top three alerts.
- [ ] The list of everything that only one person knows. Then fix that list.

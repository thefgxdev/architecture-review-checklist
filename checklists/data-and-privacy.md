# Data and privacy checklist

The engineering half of LGPD (Brazil) and GDPR (EU). The legal half stays with your lawyer; this list gives them the evidence.

## Inventory

- [ ] A list of every table, bucket, log and third-party service that holds personal data, with the fields, the purpose and the legal basis.
- [ ] Health, biometric, financial and children's data flagged as sensitive with stricter controls.
- [ ] Data flows to third parties documented: what is sent, to whom, in which country.

## Minimisation and retention

- [ ] Each personal field has a reason to exist. Fields without one are removed.
- [ ] Retention rule per category, enforced by a job, not by intention.
- [ ] Logs and backups covered by retention too. Personal data in logs is the usual leak.

## Access

- [ ] Role-based access to personal data; production access logged.
- [ ] Exports and bulk reads of personal data logged with who and why.
- [ ] Support tooling shows the minimum needed to solve a ticket.

## Rights of the data subject

- [ ] A way to find every record for one person across systems (the inventory makes this possible).
- [ ] Export in a readable format within the legal deadline.
- [ ] Deletion that reaches backups within the retention window, search indexes, caches, analytics and third parties.
- [ ] Consent records with timestamp, version of the text accepted and the channel.

## Protection

- [ ] Encryption in transit and at rest for personal data; sensitive fields encrypted at the application level when the database is shared.
- [ ] Pseudonymisation in analytics and in non-production environments. Production data never copied to development.
- [ ] Incident response plan with the notification deadline and the person responsible for the authority contact.

## Health data specifics

- [ ] Access to records tied to the treating professional and logged per access.
- [ ] Integrations with laboratories, insurers and management systems authenticated per system, with the minimum data set.
- [ ] Audit trail immutable and retained for the period the regulation requires.

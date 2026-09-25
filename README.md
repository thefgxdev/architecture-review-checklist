# Architecture Review Checklist

The questions I ask in every architecture, code and infrastructure review, and the report template I deliver at the end. By [Felipe Guedes](https://fgxdev.com), from 600+ systems reviewed or audited.

The pattern never changes: the failure is in a boundary someone trusted. These checklists walk every boundary.

## The twelve questions

1. **Where does money or an irreversible action happen?** Payments, refunds, emails, deletions, anything that leaves the system. For each one: is it idempotent? What happens if it runs twice? What happens if it runs once and the record of it is lost?
2. **What is the unit of work, and what breaks first at ten times today's rate?**
3. **Who owns each piece of data, and who else writes to it?** Two writers to one table is a bug waiting for a schedule.
4. **What happens when each dependency is down?** Not "it retries". Which features degrade, which stop, which are silently wrong.
5. **How does a request get authorised, and where can that check be bypassed?** Admin endpoints, background jobs, internal APIs, direct database access.
6. **How does a tenant's data stay a tenant's data?** Filters, cache keys, search indexes, storage paths, queues.
7. **What is the rollback for the last three deploys?** If the answer is "redeploy the previous version", was it rehearsed with the schema changes in place?
8. **When was the backup last restored, and how long did it take?** A backup that was never restored is a hope.
9. **Which secrets exist, where do they live, and when were they rotated?**
10. **What would you see first if this system started failing right now?** If the answer is "a customer would call", there is no observability.
11. **Which decisions would be expensive to reverse, and are they written down?**
12. **What does the team know that is not in the repository?** Every tribal-knowledge item is a future incident.

## Checklists

- [`checklists/code-review.md`](checklists/code-review.md): concurrency, error handling, input validation, secrets, tests that matter.
- [`checklists/infrastructure.md`](checklists/infrastructure.md): servers, cloud, network, backups, access, updates.
- [`checklists/application-security.md`](checklists/application-security.md): authentication, authorisation, injection, exposure, headers, dependencies.
- [`checklists/data-and-privacy.md`](checklists/data-and-privacy.md): personal data inventory, access, retention, LGPD/GDPR engineering controls.

## The report

Findings are useless if nobody reads them. [`report/TEMPLATE.md`](report/TEMPLATE.md) is the structure that gets read: one page of what matters, then the evidence.

Each finding has: **severity**, **evidence** (file, line, query, screenshot), **impact** (what happens, to whom), **likelihood**, **recommendation** (specific, with effort), **owner**.

Severity scale:

| Level | Meaning |
|---|---|
| Critical | Exploitable now or actively losing data or money |
| High | Will cause an incident under normal load or a routine attack |
| Medium | Will cause an incident under unusual but realistic conditions |
| Low | Quality, cost or maintainability; no incident path |

## How to run a review without offending the people who built the system

- Ask how it got this way before saying how it should be. Every strange decision had a constraint behind it.
- Findings describe the system, never the person.
- Rank by impact, not by how much you dislike the code.
- Deliver the report in a meeting, not by email.

## Em português

As doze perguntas de toda revisão de arquitetura, mais checklists de código, infraestrutura, segurança de aplicação e dados, e o modelo de relatório que é lido. Serviço de auditoria em [fgxdev.com/pt/auditoria-de-software-e-seguranca](https://fgxdev.com/pt/auditoria-de-software-e-seguranca/).

## License

Apache-2.0. Copyright (c) 2026 Felipe Guedes (fgxdev.com). Redistributions must keep the NOTICE file and mark any changes.

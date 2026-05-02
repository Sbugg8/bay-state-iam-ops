# Bay State Regional Health — IAM Operations Lab

A simulated identity and access management environment for a fictional ~250-person regional health system in Massachusetts. This repo is the operational layer (tickets, runbooks, standups, audit evidence) wrapped around a real Okta dev tenant.

**This is a learning lab.** The company, users, and tickets are fictional. The Okta tenant, group rules, lifecycle workflows, and operational processes are real. The goal is to operate IAM the way a junior analyst at a Massachusetts health system would, and document that work end-to-end.

## Stack

| Layer | Tool |
|---|---|
| Identity provider | Okta (Workforce Identity Cloud) |
| Apps integrated | Microsoft 365, Slack, ServiceNow, Epic EHR, Workday, Tableau |
| Ticketing | GitHub Issues (Phase 1) → ServiceNow PDI (Phase 2) |
| Documentation | Markdown in this repo |
| Communications | Internal Slack workspace (`bsrh-iam.slack.com`) |

## Compliance scope

HIPAA, HITECH, MA 201 CMR 17. Quarterly user access reviews (UARs), 30-day deactivation window on terminations, MFA required on all clinical and administrative apps.

## Repo structure

- [`company/`](company/) — org chart, app access matrix, group structure, lifecycle policies
- [`standup/`](standup/) — daily three-line written standups
- [`runbooks/`](runbooks/) — operational procedures (joiner, mover, leaver, MFA reset, incident response)
- [`tickets/`](tickets/) — ticket templates and references (live tickets are GitHub Issues)
- [`evidence/`](evidence/) — UAR pulls, audit artifacts, attestation logs

## Daily ops

Triage → standup → execution → documentation. Standups committed every weekday by 8:30 AM.

## Maintained by

Genius Bugg — Junior IAM Analyst (sim)

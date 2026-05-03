# Runbook: Clinical RN onboarding (joiner)

**Trigger:** Workday creates new hire record with `department = "Clinical"` and `title` containing "RN" or "Nurse"
**Owner:** IAM Analyst on call
**SLA:** Account active and verified by 8:00 AM on user's start date

## Pre-checks

- [ ] Workday record contains: legal name, work email, manager, department, title, start date
- [ ] HR has confirmed I-9 complete and clinical credentialing verified
- [ ] Manager has filed access ticket for any non-standard apps (e.g. specialty modules in Epic)

## Provisioning steps

1. Verify user appears in Okta via Workday HR-as-master profile push (typically within 1 hour of Workday creation). Path: Okta admin → Directory → People → search by name.
2. Confirm group rule `bsrh-clinical` evaluated and added user to group. Path: Directory → Groups → `bsrh-clinical` → Members.
3. Verify automatic app assignments inherited from group membership:
   - Microsoft 365 (via `bsrh-all-employees`)
   - Slack (via `bsrh-all-employees`)
   - ServiceNow (via `bsrh-all-employees`)
   - Epic EHR (via `bsrh-clinical`)
4. Send Okta activation email to user's personal email on file.
5. Tag the corresponding ticket with labels: `joiner`, `clinical`, and link the Workday req number in the ticket body.

## Day-1 verification (run on user's start date)

- [ ] User completed MFA enrollment (Okta Verify or WebAuthn — SMS not permitted for clinical)
- [ ] User has logged into M365 at least once
- [ ] User has logged into Epic at least once (clinical-readiness check)
- [ ] No failed-login alerts on first login attempt

## Common issues

**Workday push delay >2 hours.** Check Workday HR-as-master integration health: Okta admin → Workflow → Integrations → Workday → run history.

**Epic access denied despite group membership.** Group assignment is correct in Okta but Epic-side role mapping is the gating factor. Reach out to Epic admin (clinical IT) with username, department, and intended Epic role.

**MFA enrollment skipped or expired.** User can self-enroll at `<tenant>.okta.com/enroll/factors`. If the enrollment window has lapsed, send a new activation email and document the delay in the ticket.

## Close-out

- [ ] User confirms ability to access M365, Slack, and Epic
- [ ] Manager confirmed onboarding complete in Slack DM
- [ ] Ticket closed with comment: "Provisioning complete — apps verified day-1, MFA enrolled."

## Evidence to retain

- Screenshot of group membership in `bsrh-clinical` (timestamp visible)
- System Log entry for first successful login
- MFA factor enrollment confirmation

Saved under: `evidence/joiners/YYYY-MM-DD-<lastname>-<firstinitial>.md`

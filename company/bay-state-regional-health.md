# Bay State Regional Health

## Profile

- ~250 employees across 5 departments (sample roster of 18 in this lab)
- Headquartered in Boston, MA (fictional)
- Industry: Healthcare — acute care and outpatient services
- Compliance scope: HIPAA, HITECH, MA 201 CMR 17

## Departments

| Department | Sample headcount | Department lead | Standard app set |
|---|---|---|---|
| Clinical | 5 | Dr. James Patel, CMO | M365, Slack, ServiceNow, Epic EHR |
| IT | 4 | Priya Shah, IT Director | M365, Slack, ServiceNow |
| Finance | 3 | Sarah O'Brien, Finance Manager | M365, Slack, ServiceNow, Workday, Tableau |
| HR | 3 | Karen Sullivan, HR Director | M365, Slack, ServiceNow, Workday, Tableau (Director only) |
| Facilities | 3 | Joseph Romano, Facilities Manager | M365, Slack, ServiceNow |

## Application access matrix

| App | Clinical | IT | Finance | HR | Facilities |
|---|---|---|---|---|---|
| Microsoft 365 | ✓ | ✓ | ✓ | ✓ | ✓ |
| Slack | ✓ | ✓ | ✓ | ✓ | ✓ |
| ServiceNow | ✓ | ✓ | ✓ | ✓ | ✓ |
| Epic EHR | ✓ | | | | |
| Workday | | | ✓ | ✓ | |
| Tableau | | | ✓ | Director only | |

## Okta groups

| Group | Purpose | Membership |
|---|---|---|
| `bsrh-all-employees` | Baseline access (M365, Slack, ServiceNow) | All active users |
| `bsrh-clinical` | Clinical apps (Epic) | `department == "Clinical"` |
| `bsrh-it` | IT tooling | `department == "IT"` |
| `bsrh-finance` | Finance apps (Workday, Tableau) | `department == "Finance"` |
| `bsrh-hr` | HR apps (Workday) | `department == "HR"` |
| `bsrh-facilities` | Facilities | `department == "Facilities"` |
| `bsrh-managers` | Manager-only resources | Title contains "Manager" or "Director" |
| `bsrh-tableau-users` | Tableau access | Finance team + HR Director |

## Okta group rules

Configure these as dynamic group rules so membership updates automatically when the `department` or `title` attribute changes:

| Rule name | Expression |
|---|---|
| Assign Clinical | `user.department == "Clinical"` |
| Assign IT | `user.department == "IT"` |
| Assign Finance | `user.department == "Finance"` |
| Assign HR | `user.department == "HR"` |
| Assign Facilities | `user.department == "Facilities"` |
| Assign Managers | `user.title.contains("Manager") OR user.title.contains("Director")` |
| Assign Tableau | `user.department == "Finance" OR (user.department == "HR" AND user.title.contains("Director"))` |

## Identity lifecycle policy

**Joiner.** HR creates new hire in Workday → Workday HR-as-master profile push to Okta → group rules assign apps automatically → MFA enrollment required within 7 days of activation.

**Mover.** Department or title change in Workday → group rules re-evaluate within 1 hour → access adjusts automatically → manager attestation required within 14 days for any access additions.

**Leaver.** Termination filed in Workday → Okta deactivation within 1 hour → all sessions revoked → 30-day suspension period → permanent account deletion at 90 days.

## MFA policy

| Group | Factor requirement |
|---|---|
| `bsrh-all-employees` | At least one factor (Okta Verify, WebAuthn, or SMS as fallback) |
| `bsrh-clinical` | Okta Verify or WebAuthn (no SMS — PHI access) |
| `bsrh-managers` | Okta Verify or WebAuthn |
| Admin console access | WebAuthn only |

# Cron Runbook

We track cron *definitions* in git, and apply them to the running OpenClaw gateway via `cron.add` / `cron.update`.

## Jobs

- `indeed-triage` (multiple times/day): visits Indeed query pages in the OpenClaw browser profile, de-dupes by `jk`, and appends new postings to `data/tracker.csv` with a short JD-based summary.
- `morning-apply` (daily): reads `data/tracker.csv` for `apply` entries and drives applications. Requires human intervention for CAPTCHAs/2FA/company-site logins.

## Install/update

1) Edit the JSON job definitions in `cron/`.
2) Apply them to the gateway.

Notes:
- Indeed blocks scraping; this is browser-driven automation.
- Never commit secrets.

*Footnote: After cron updates merge to `main`, run `cron.list` to obtain the job IDs and `cron.update` (or `cron.add` for new entries) so the live gateway matches the repo definitions. Repeat `cron.list` afterward to verify the gateway state.*

# Cron Runbook

We track cron *definitions* in git, and apply them to the running OpenClaw gateway via `cron.add` / `cron.update`.

## Jobs

- `indeed-triage` (multiple times/day): visits Indeed query pages in the OpenClaw browser profile, de-dupes by `jk`, and appends new postings to `data/tracker.csv` with a short JD-based summary.
- `morning-apply` (daily): reads `data/tracker.csv` for `apply` entries and drives applications. Requires human intervention for CAPTCHAs/2FA/company-site logins.

## Install/update

1. Edit the JSON job definitions in `cron/` (and update the recorded browser snapshot/actions if the UI changed).
2. Push the branch, open a PR against `main`, and once it merges make sure the running system cron is aligned with the merged definitions.
3. After merging, run `cron.list` to find the job IDs and use `cron.update` (or `cron.add` if the job is new) so the gateway uses the latest payloads.
4. Confirm `cron.list` again and, if the job logs snapshot refs, re-capture a fresh snapshot whenever the page layout shifts (so the automation does not rely on stale refs such as `e12`).

## Notes

- Indeed blocks scraping; this is browser-driven automation.
- Never commit secrets.

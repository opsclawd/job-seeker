# Indeed Backfill (Manual Batch)

Purpose: crawl deeper into the last-30-days backlog in controlled batches.

## Batch size
- **10 pages per query per run** (default)

## How to run (manual)
In chat, tell Clawd:
- **"backfill batch"**

The runner should:
1) Read `streams/indeed/QUERIES.md`
2) Load `data/tracker.csv` (de-dupe by `id` = `jk`)
3) Load/update `data/backfill-state.json`
4) For each query, start from the saved `start` offset and process **10 pages**.
   - Extract each job `jk`
   - Open `viewjob?jk=...` in the OpenClaw browser profile
   - Write a JD-based summary into `notes`
   - Set `status`: apply/new/maybe/skip (manager roles=maybe; Salesforce-only=skip; DevOps-heavy/SAP-only/unrelated=skip)
5) Commit + push if tracker changed
6) Send Telegram summary + note if any verification/CAPTCHA blocked progress

## Notes
- Indeed is Cloudflare-protected: **browser-only**, no `web_fetch`.
- Keep rate-limits conservative; stop on verification.

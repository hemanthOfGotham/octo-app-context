# Octo analytics context

Nao agent context for Fello's Octo data — the `octo-postgres` database (Railway,
private network) exposing the analytics tables only.

## Tables (public schema)

**Felix Sentiment** (per-account AI sales-agent health):
- `felix_accounts` — account universe (name, hubspot_id, status, kpis jsonb, snapshot jsonb)
- `felix_account_signals` — cheap per-account signals (score, band, gong_felix_calls, gong_pos/neg, mrr, arr, plan)
- `felix_account_detail` — LLM deep-dive (verdict, confidence, narrative, findings, friction, risks, key_moments)
- `felix_score_history` — weekly score snapshots (for trends)
- `felix_coaching_scan` — portfolio Asking-vs-Prescribing scan
- `felix_account_signals.account_id` / `felix_account_detail.account_id` → `felix_accounts.id`

**Gong** (synced call data):
- `gong_calls` — mirrored Gong calls (title, started, parties jsonb, account_names, transcript, url)
- `gong_sync_state` — sync cursor

## Routing rules
- Account health / sentiment / score / band questions → `felix_account_signals` + `felix_account_detail`, joined to `felix_accounts` by id.
- "Testimonial-ready / at-risk / watch" = `felix_account_signals.band`.
- Revenue (MRR/ARR) → `felix_account_signals.mrr`/`arr`, or `felix_accounts.kpis` jsonb.
- Call volume / Felix mentions → `gong_calls` (filter `started >= '2026-05-11'` for post-launch) + `felix_account_signals.gong_felix_calls`.
- Coaching style (asking vs prescribing) → `felix_coaching_scan`.
- Read-only. Never write.

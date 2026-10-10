# Voice Receptionist -> AI OS integration plan (DRAFT, source-unverified)

Status: plan only. `ADDON-PLAN.md`, `SETUP-CHECKLIST.md` and voice source were NOT available. Every "Muse reports" claim is unverified.

## Target flow
Incoming call -> Retell agent -> **authenticated webhook** -> tenant-specific lead record -> AI OS CRM -> booking-link SMS (only if consent) -> owner notification -> monthly report.

## Design requirements
- **Tenant routing:** map dialed number (or Retell agent ID) -> tenant ID server-side; never trust a tenant ID from the payload. Unknown number = reject + log.
- **Webhook auth:** verify Retell signature on the raw body before parsing; reject stale timestamps; secret only in env var (`RETELL_API_KEY`-style names, values never in git).
- **Idempotency:** unique key on (tenant, provider call_id, event type); duplicate = 200 no-op. Retries from Retell are expected.
- **Audit:** append-only event log per tenant (received, verified, routed, SMS decision, owner notified) with timestamps; no secrets in logs.
- **Lead write:** insert/upsert lead with source=`voice`, attribution, timestamp; link call record. Reuse the AI OS leads table; do not create a parallel store.
- **SMS gate:** send only if tenant has an active A2P registration, caller consent is captured/valid for this purpose, caller is not on opt-out list, and quiet-hours rules pass. Honor STOP/HELP. Default = **off**; founder enables per tenant.
- **Transcripts/PII:** encrypt at rest, tenant-scoped access, retention limit, redact in logs, deletion on request. Disclose AI + recording at call start per state law.
- **Failure handling:** queue webhook work; retry with backoff; dead-letter after N tries; alert owner. Provider outage -> fallback voicemail + text-to-owner (no SMS to caller without consent).
- **Usage/billing:** store per-call duration per tenant; monthly rollup vs 1,000 included minutes. Metering only; **no charging, no Stripe products** without approval.
- **Off by default:** no phone forwarding, no number purchase, no activation.

## Verification checklist (to run once source is supplied)
| Question | Answer |
|---|---|
| Standalone Python service? | UNVERIFIED |
| Where webhooks are hosted | UNVERIFIED |
| Tenant isolation | UNVERIFIED |
| Retell signature verified? | UNVERIFIED |
| Twilio creds secured? | UNVERIFIED |
| Duplicate webhook handling | UNVERIFIED |
| SMS consent / opt-out | UNVERIFIED |
| Transcript/PII protection | UNVERIFIED |
| Retry / outage handling | UNVERIFIED |
| Billing/usage measurement | UNVERIFIED |
| How AI OS stores leads | UNVERIFIED (needs empire-web access) |
| Tests present and passing | UNVERIFIED |

Pricing is a proposal held by the founder; not recorded here (public repo).

# Changelog

## v1.0 — 2026-11-08

First public release (GNU GPL v3).

- Platform-neutral build of the Scam Detector skill: no host, vendor or personal-system assumptions. A new opening section — "Channel access: inventory your own capabilities first" — has the installing agent map the channels it can genuinely reach (connected mailbox, X posts/DMs, Messenger, WhatsApp, Telegram, any connected system) and screen through those, with honest content-only fallback for the rest.
- Screening procedure: sender authentication (SPF/DKIM/DMARC + alignment, lookalike domains, anchor domains), account-age and tenure checks, link/URL/QR analysis as text, content and social-engineering analysis, attachment metadata screening, external reputation checks — each logged as run or not-run.
- Social-scam layer: classification by the nature of the message (romance, pig-butchering, impersonation/emergency, authority plays, job/task, windfall, sextortion, marketplace), with an earmark-and-warn rule and trajectory staging.
- WhatsApp module: header-less identity checks, compromised-account rule, platform-shaped asks, chat-export screening.
- Five verdict bands with cross-channel hard floor rules; fixed verdict output format.
- Self-updating threat-intel reference file (dated, sourced, append-only), seeded with Singapore anchors and advisories current at 5 Nov 2026.

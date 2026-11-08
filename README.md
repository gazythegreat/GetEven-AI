# Scam Detector
**Get it:** [geteven.ai/detector](https://geteven.ai/detector) — what it checks, the verdict bands, and the free download.

An agent skill (SKILL.md format) that screens a message — email, X (Twitter), Facebook/Messenger, Instagram/Threads, WhatsApp, Telegram, SMS/iMessage, or anything pasted in — and returns an **evidence-based scam verdict**.

It is **host-neutral and capability-adaptive**: it runs inside any AI agent that supports SKILL.md skills (Claude, ChatGPT, Grok, Muse, and others), and its first step is to inventory the channels *that agent* can actually reach — a connected mailbox, X posts and direct messages, a messaging account — and screen through those, falling back to content-only screening of pasted material for everything else. It states plainly what it could not check, and never claims a check it did not run.

## What it checks

- **Sender authentication** — SPF/DKIM/DMARC pass/fail *and alignment*, display-name/address mismatch, lookalike-domain analysis (edit distance, homoglyphs, punycode, brand-as-subdomain), known-good anchor domains for claimed authorities.
- **Account age and tenure** — platform join dates where served (Facebook Page Transparency, Instagram "About this account", X "Joined"), labelled proxies where not (WhatsApp serves no creation date at all).
- **Links, URLs and QR codes** — analysed as text, never visited: visible-text vs destination mismatch, shorteners, IP hosts, punycode, consent/device-code OAuth lures, quishing.
- **Content and social engineering** — the ask is classified (credentials, OTP, payment to new details, callback, install, secrecy, deadline) and the pressure stack weighed; BEC/payment-redirection pattern; trajectory over prose quality.
- **A social-scam layer** — detection by the *nature* of the message: romance scams, pig-butchering/investment mentorship, family/friend impersonation and emergency plays, authority impersonation, job/task scams, windfall plays, sextortion, marketplace plays. Matches are **earmarked and warned immediately**, before technical corroboration, with trajectory staging for conversation arcs.
- **Attachments** — metadata only, never opened: high-risk types, double extensions, password-protected archives that supply their own password.
- **External reputation** — official advisories (SPF, CSA), ScamShield, Safe Browsing, VirusTotal, urlscan.io, PhishTank, Spamhaus, WHOIS domain age, brand cross-checks — each logged in the verdict as run or not-run.

## Verdicts

One of five bands, with confidence and the evidence grouped by sender / links / content / attachments / external checks:

`CONFIRMED SCAM` · `LIKELY SCAM` · `SUSPICIOUS — verify independently` · `LIKELY GENUINE` · `UNVERIFIED`

Hard floor rules apply across all channels (e.g. authority impersonation from a lookalike or free-mail domain is at least LIKELY SCAM; a WhatsApp verification-code ask is at least LIKELY SCAM). The exact output shape is in [`references/verdict-format.md`](references/verdict-format.md).

## Safety model

- **Read-only.** The skill never clicks a link, opens an attachment, scans a QR code, replies, or calls a number printed in a message. It never deletes or alters a screened message.
- **Lookups leak nothing personal** — only the suspect domain, URL, phone number or file hash is submitted to reputation sources.
- **Honesty about access.** Several official services (ScamShield included) offer human-facing checkers but **no public API**. Where the host cannot query a service, the skill says so and routes the user to run that check themselves — a database is never reported as "checked" when it wasn't.

## Install

1. Clone or download this repository.
2. Copy the folder into your agent's skills directory (for Claude, e.g. `~/.claude/skills/scam-detector/`; other hosts: wherever that host loads SKILL.md skills from).
3. Ask your agent: *"Is this a scam?"* and paste a message, or point it at a message in a channel it can reach.

Full capability (Steps 5–6) needs a host with web search; without it, the skill still runs content, link-as-text and social-play screening and marks the skipped checks honestly.

## Files

- `SKILL.md` — the skill itself: standing rules, capability inventory, the WhatsApp module, the social-scam layer, the six-step procedure, scoring and hard rules.
- `references/verdict-format.md` — the exact verdict output format.
- `references/indicators-and-techniques.md` — the living threat-intel file: known-good anchors, indicators by channel, dated technique changelog. The skill's Step 6 refresh pass appends new dated, sourced entries and never deletes old ones.

## Jurisdiction

The seeded anchors and advisories are **Singapore** (SPF / Anti-Scam Centre, ScamShield, IRAS, CSA) — that is where the skill was built and where its patterns are sharpest. The procedure itself is jurisdiction-neutral; extend the anchor list in the references file for your own country.

## Updates

This repository is published in snapshots. The skill also carries its own refresh pass (Step 6): an installed copy appends newly observed techniques and advisories to its local references file, dated and sourced, so a working copy keeps learning between releases.

## Licence

GNU General Public License v3.0 — see [`LICENSE`](LICENSE). Use it, adapt it, share your improvements under the same terms.

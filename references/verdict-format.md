# Verdict format — Scam Detector

Every screening result is reported in this shape. Verdict first, evidence after, no buried ledes.

---

**Verdict: <CONFIRMED SCAM | LIKELY SCAM | SUSPICIOUS — verify independently | LIKELY GENUINE | UNVERIFIED>** (confidence: high/medium/low)
Message: <channel> · from <sender as shown> · <subject/first words> · <date/time if known>

**What it asks for:** <the classified ask — click / credentials / OTP / consent / device code / payment to new details / callback / QR scan / data reply / nothing>

**Evidence**
- Sender: <authentication result, domain analysis, profile signals — each marked (observed) or (inferred)>
- Links: <each destination vs its label; QR noted as unexaminable where applicable>
- Content: <pressure stack, pattern matches to the indicators file/advisories>
- Attachments: <types and flags, or "none">
- External checks: <each source run and its result; each source NOT run and why>

**What I could not check:** <gaps — e.g. pasted content has no headers; database needs an API key we do not hold>

**What to do:** <independent verification route with contact points found independently of the message; reporting routes if scam (ScamShield 1799 / scamshield.gov.sg, the impersonated brand's phishing address, the platform's report function); what NOT to do, stated once>

---

Rules:
- If a hard rule in SKILL.md set the floor verdict, name the rule.
- A LIKELY GENUINE verdict must still list residual risks and any check that was skipped.
- Batch screenings use the same block per message, shortened only by omitting empty sections; the verdict line is never omitted.

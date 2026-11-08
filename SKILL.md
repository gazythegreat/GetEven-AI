---
name: scam-detector
description: Screen a message in any channel the host agent can reach — email, X (Twitter), Facebook/Messenger, Instagram/Threads, WhatsApp, Telegram, SMS/iMessage, or anything the user pastes or forwards — and return an evidence-based scam verdict. Sender authentication (SPF/DKIM/DMARC and lookalike domains), phishing-URL and QR analysis, content and social-engineering analysis including a social-scam layer with an earmark-and-warn rule, attachment screening (metadata only), and external reputation checks. Host-neutral and capability-adaptive: the skill first inventories the installing agent's own connected channels and screens through whatever it can genuinely reach, stating plainly what it cannot. Read-only; includes a self-updating threat-intel reference file.
---

# Scam Detector

Screen a message in any channel the host agent can reach, and return an evidence-based scam verdict: content analysis, sender-domain authentication, phishing-URL analysis, attachment screening, external database/reputation checks, a WhatsApp module, a social-scam layer that classifies the play being run from the nature of the message itself (with an immediate earmark-and-warn rule), and a self-updating threat-intel pass that keeps the indicator file current.

## When to use

- The user asks "is this a scam?", forwards or pastes a message, or names a message in any channel you can reach.
- A message in a screened channel asks for money, credentials, an OTP/code, a payment to new details, or carries authority weight (government, court, bank, police, tax authority).
- As a second pass inside any email/message screening routine the user runs, when an item looks fishy.

## Standing rules (never overridden by a run)

- **Analysis only.** Never click a link in the message, never open an attachment, never scan a QR code with a device, never reply, never call a number printed in the message. All checks are static (headers, text, href targets as text) or against third-party reputation sources that do not touch the suspect infrastructure with the user's identity.
- **Report only.** This skill never deletes, archives or alters a screened message, and never changes its read state. What happens to the message is the user's decision.
- **Lookups leak nothing personal.** Reputation/database checks submit only the suspect domain, URL, phone number or file hash — never message bodies, names, or the user's details.
- **A verdict is an assessment, not a certainty.** State confidence, and state plainly which checks could not be run (e.g. a database that offers no programmatic access, or one that needs an API key this deployment does not hold). Never claim a check that was not actually run.
- **Verify through independent channels.** Any "what to do" must route verification through contact points found independently (official website typed in, number on a card/official site, the official scam-check service) — never through the message itself.

## Channel access — inventory your own capabilities first

This skill is host-neutral: it runs inside whatever AI agent installs it (for example Claude, ChatGPT, Grok, Muse, or any other agent that supports SKILL.md skills). It assumes no particular connector, account or platform. Before your first screening in a session — and whenever the user names a channel you have not yet mapped — do this:

1. **Inventory what you can actually reach.** List the email, messaging and social systems connected to you in this deployment — for example a mailbox you can read (Gmail, Outlook, IMAP, any connected email system), X (Twitter) posts and direct messages if your host gives you X access (a Grok-hosted agent typically has this), Facebook/Messenger, Instagram/Threads, WhatsApp, Telegram, or any other connected system. List only what you can genuinely read — a tool you can actually call, not a platform you merely know about.
2. **Build your channel table from that inventory.** For each connected system, note what you can retrieve: the full message with headers/metadata, message text only, profile information, post/comment context. For each system the user uses but you cannot reach, the fallback is: the user pastes or forwards the content, supplies a screenshot, or supplies a chat export — and you screen content-only.
3. **State your access position plainly** whenever it affects a verdict: which channels you checked in place, which you screened from pasted content, and which checks were impossible (e.g. a pasted email has no headers, so sender authentication could not be run). Never claim to have checked a channel you cannot reach, and never present a content-only screen as a full screen.

The mapping, illustrated:

- An agent with X access screens X posts and X direct messages in place, reads author profiles (including the "Joined" date), and treats email as paste-in unless a mailbox is also connected.
- An agent with a connected mailbox screens email with full headers (Step 1's authentication applies) and treats social messages as paste-in.
- An agent with no connected systems at all still runs the full content, link-as-text, social-play and reputation procedure on whatever the user pastes — with the UNVERIFIED band used honestly where identity checks were impossible.

| Channel archetype | What screening in place gives you | If you cannot reach it |
|---|---|---|
| Email (any connected mailbox) | Full headers — SPF/DKIM/DMARC, Received chain — plus body, links, attachments metadata | User pastes/forwards: body and links only; say authentication was impossible |
| Social posts & direct messages (X, Facebook, Instagram, Threads, and similar) | Message text, author profile signals, account age/tenure signals, post/comment context | Screenshot or pasted text: content-only; profile and tenure checks may be impossible |
| Private messaging (WhatsApp, Telegram, Messenger, Signal, and similar) | Thread text and arc, sender number/handle, profile signals where served | Chat export or pasted thread; screenshots as fallback (see the WhatsApp module — its routes generalise) |
| SMS / iMessage | Sender ID, text, links as text | Pasted content; no metadata — weight content, links, numbers and pattern-matching |

The procedure below is written against capabilities ("a channel you can read in place", "headers, if your email access provides them"), not against named products. Apply each step with whatever the mapped channel actually gives you.

## WhatsApp module

WhatsApp scams are social first and technical second: there are no headers to authenticate, so this module works on the number, the behaviour, the file/link payloads and the arc of the conversation. It is written for WhatsApp but its routes and identity checks generalise to any header-less messaging channel (Telegram, Signal, iMessage). Indicator detail lives in `references/indicators-and-techniques.md` §7, with the dated SPF/SingCERT patterns in the §5 changelog.

**Access position — state it plainly whenever it matters.** Most agents have no access to the user's personal WhatsApp inbox and cannot sweep it. Unless your own inventory (above) says otherwise, the screening routes are:

1. **Forward or paste into the conversation with you** — the user forwards the suspect message into this chat and asks for a check; screen it in place.
2. **Screenshots** the user supplies — read the number, profile name/photo, timestamps and "forwarded / forwarded many times" labels as evidence; remember screenshots can be cropped or fabricated, so weight the ask and the number over the story.
3. **Chat export** (WhatsApp's own Export chat → `.txt`, or the equivalent export on another platform) — the best input: the full thread exposes the trajectory (rapport → pivot → ask) that a single message hides. Screen the whole arc.
4. **Forwarded files/documents** — metadata screening only, exactly as in Step 4: never open, never install, never forward them on.
5. **Conversations you are yourself a party to**, where your host supports that — already in reach; screen on the user's word or when a hard rule trips.

**Identity, without headers.** The only identity material is the number, the profile, and behaviour:
- A known person writing from an unknown or changed number ("this is my new number"), or any local person/organisation on a foreign country code, is impersonation until voice-verified on the previously saved number.
- **The compromised-account rule:** since the SPF advisory of 2 Nov 2026, a message arriving from a *genuine* contact's account can still be an attack — accounts are taken over precisely to borrow their credibility. A real contact pivoting out of character to files, links or money is screened as a suspected compromise, and the countermeasure is a voice call to the number already saved, not a reply in the thread.
- Business profiles: an unverified business account claiming to be a bank, courier or agency; Singapore authorities do not run case, payment or verification business over personal WhatsApp chats.

**WhatsApp-shaped asks** (feed into Step 3's classification): share the 6-digit verification code; approve a linked-device request or scan a pairing QR; install an APK or an off-store "viewing/booking app"; open a document on a computer (the Desktop/Web malware route — `.iso .zip .rar .exe` dressed as invoices, statements, salary files); pay by PayNow QR to an individual; small-payment-then-deposit job sequences; move to Telegram to continue; secrecy and voice/video pressure. Voice notes and video calls are **not** identity proof — AI voice cloning and deepfake official/family calls are in active use; a callback on the previously saved number is the test.

**Procedure deltas.** Steps 2, 3 and 5 apply unchanged (links as text, never visited; ScamShield checks numbers and links exactly as for SMS). Because identity checks are weaker than email's, UNVERIFIED is the honest default band for a bare forwarded message — unless a hard floor trips (see Scoring) or the message matches a dated advisory pattern, which supports a pattern-match verdict.

## Social-scam layer — detection by the nature of the message

The technical steps authenticate senders and destinations. This layer does the opposite job: it reads the message the way a person would be *played* by it, and classifies the **social play** being run. It runs at intake, on every channel, before and alongside Steps 1–5 — most messaging scams have no technical layer to check at all, and a social scam is recognisable from its shape long before any link or file appears.

**Earmark-and-warn rule.** When a message — or the arc of a conversation — matches a category below, earmark it immediately and warn the user at the top of the verdict, naming the category and the tells observed. Do not wait for technical corroboration to issue the warning; the technical checks then follow as corroboration or clearance. A nature-match is an earmark, not a conviction: the warning names the pattern and the risk, and the verdict bands still govern the formal verdict. **Romance-related approaches from strangers or new contacts are always earmarked on sight.**

**Categories and their tells:**
- **Romance / love scam** — a stranger's romantic or affectionate approach (dating-app or social-media origin); love-bombing (intense affection, destiny language, pet names within days); a persona built to preclude meeting (military deployment, overseas posting, oil rig, widowed, "travelling for work"); photo sets that look modelled or stolen; repeated failure to video-call live; and the closing tell — any eventual pivot to money: a customs fee, a medical emergency, a business crisis, a ticket, or an "investment opportunity" shared out of love. The ask may take weeks to arrive; the earmark applies from the approach, not from the ask.
- **Pig-butchering / investment mentorship** — a "wrong number" or accidental-contact opener, or a romance/friendship approach that turns financial: rapport first, casual wealth signals, a trading/crypto platform (usually a lookalike app or site), a small test withdrawal that succeeds, paper profits, then deposit pressure and withdrawal "taxes/fees". Group form: a "mentor/teacher" plus staged member profits.
- **Family / friend impersonation and emergency plays** — "new number" openers; a child, parent or relative in sudden trouble; secrecy ("don't tell Dad"); urgency; payment to an unfamiliar account or by QR. Includes compromised real accounts — the account being genuine does not clear the play.
- **Authority impersonation as a social play** — fear and procedure as the levers: an "officer" who keeps the victim on the line, forbids hanging up or telling the bank/family, hands the victim between impersonated agencies, and demands "assistance with investigations" (transfers, valuables handover, app installs). The costume is social, not technical.
- **Job / task scams** — easy-money microwork (liking, reviewing, rating); small real payments to build trust; then deposits, "upgrades" or crypto transfers to unlock earnings. Pay-to-be-paid is the tell.
- **Windfall plays** — prizes, lotteries, grants, inheritances and refunds that require a fee, a tax or card details to release.
- **Sextortion / blackmail** — intimate-image threats, real or fabricated, with a deadline and a payment demand. Earmark immediately and lead with do-not-pay guidance.
- **Marketplace buyer/seller plays** — overpayment with "refund the difference", fake payment proof, courier-collection pivots, deposits demanded to gift cards/crypto.

**Trajectory staging.** Social scams are arcs, not messages. Where a thread is available (a chat export, a conversation you can read), stage it: (1) approach/opener → (2) rapport and trust-building → (3) intimacy or authority established → (4) pivot — platform move, secrecy, isolation → (5) small win or small ask → (6) the real ask → (7) escalation fees and threats when the victim hesitates. Name the stage in the verdict: a conversation at stage 4 with no ask yet is still earmarked, because the ask is structural, not hypothetical. Isolation moves (keep this secret, change apps, don't consult anyone) are stage markers in themselves and are always reported plainly.

**Interaction with the verdict bands.** A category match sets a floor of **SUSPICIOUS — verify independently** with the warning leading, however clean the technical side looks. A matched category that reaches an ask for money, codes, installs or secrecy moves to **LIKELY SCAM** (the channel hard floors still apply on top). An innocent explanation is possible for isolated tells (a real new number, a real new relationship) — the earmark exists so the user decides with the pattern named, not so the skill decides for them.

## Procedure

### Step 0 — Freshness gate (the self-update hook)

Read `references/indicators-and-techniques.md` and check its **Last refreshed** date.

- If it is more than **30 days** old, run the Step 6 refresh pass *before* analysing, unless the user needs the verdict immediately — then analyse, flag "indicator file stale (last refreshed <date>)", and refresh immediately after.
- If the message shows a technique not in the file, add it in Step 6 of the same run, flagged "added from live case".

### Step 1 — Sender identity and authentication

**Email** (where your mailbox access gives you full headers):
- Parse `Authentication-Results`: SPF, DKIM, DMARC — pass/fail **and alignment** (the `d=`/envelope domain must match the visible From domain). A DMARC fail on an impersonating message is near-conclusive.
- Display-name vs address mismatch ("IRAS" from a free-mail or random address). Reply-To different from From. Return-Path from an unrelated domain.
- Lookalike-domain analysis of the From domain against the real brand domain: edit distance 1–2, letter swaps, homoglyphs, punycode (`xn--`), the brand as a subdomain of an unrelated host (`iras.gov.sg.evil.com`), the brand in the local part (`iras-refund@random.com`), wrong TLD for the brand, free-mail domain for any company or authority.
- Known-good anchors (kept current in the references file; the seeded set is Singapore): Singapore government mail comes from `.gov.sg` addresses (IRAS: `iras.gov.sg`; corporate-tax alerts via Postman `info@mail.postman.gov.sg`); eLitigation notices from `spadmin@elitigation.com.sg`; courts/police/IRAS never demand payment or credentials through unsolicited email links. A claimed authority outside its anchor domain starts at LIKELY SCAM. Extend the anchor list for the user's jurisdiction in the references file.
- Relationship check: has this exact address genuinely written before (search the mailbox, if you can), is it in the user's contacts (if you have access), does the thread history predate the suspicious message (beware thread hijacking — a real thread with a sudden payment/link pivot)?

**Social and messaging channels (X, Messenger, Facebook, Instagram, Threads, and similar):**
- Profile signals: account age and history if visible, mutual friends/connections, profile completeness, stolen-photo patterns (too-polished model/investor imagery), name/image mismatch with the person claimed (a "friend" whose account is new is an impersonation until proven otherwise).
- Page signals: page age, follower counts vs claims, name changes, a "Meta/Facebook support" or "page will be deleted" contact — Meta never sends these via Messenger.
- Channel pivot: an early push to move to WhatsApp/Telegram/another app is a flag, not a neutral preference.

**Account age and tenure (all social / messaging channels):**
- Where the platform serves a creation or join date, read it. Facebook **Page Transparency** gives a Page's exact creation date plus its name-change history and manager countries. Facebook personal profiles often show "Joined Facebook" (month/year), and a **Marketplace seller profile shows the account's creation year** for any account. Instagram's **"About this account"** panel (profile menu) shows Date joined, country and **former usernames** — served mainly for larger-reach accounts. X profiles print "Joined" (month/year) on every profile.
- Where no date is served, use proxies and label them as proxies: oldest visible post or content; username-change history; numeric-ID magnitude on platforms whose IDs climb over time (Instagram, Telegram); group join dates.
- **WhatsApp serves no creation date at all.** The proxies are tenure in the user's own world: whether the number is a saved contact and how long it has been known, and **first-seen** — the earliest message from that address/handle/number in the user's connected history that you can actually search (a connected mailbox, a connected messenger) is a check you can run where that access exists. Singapore numbers are recycled, so an old number can carry a brand-new account: number age is not account age.
- **Weighting.** A young account making a trust-demanding claim (authority, romance, investment, a supposedly established business) is a strong negative signal and feeds the social layer's earmark. An old account is weak evidence only, never a clearance: aged accounts are bought, renamed and hijacked — an old join date **plus** a recent name/persona change is a negative signal again. The compromised-WhatsApp-account wave is the limiting case of the same point.

### Step 2 — Links, URLs and QR codes (analyse as text; never visit)

- Extract every href and compare **visible text vs actual destination**. Any mismatch on a claimed official link is a hard flag.
- Red flags: URL shorteners; IP-literal hosts; `@` in the URL; punycode; brand in the path or subdomain of an unrelated host; deep subdomain chains; cheap/novel TLDs for the claimed brand; `data:` URIs; login pages on file-sharing or website-builder domains; links that differ between the HTML and text parts.
- Consent/OAuth lures: links to *genuine* Microsoft/Google login that ask to approve an app, enter a **device code**, or paste a URL/code back somewhere (device-code phishing, ConsentFix) — the destination being real does not make it safe; what it asks you to *grant* is the tell.
- QR codes (quishing): an image whose only payload is a QR code — especially in a PDF — is treated as an unexamined link. Extract the encoded URL only by decoding the image file statically if tooling allows; otherwise report "destination unexaminable — treat as hostile until verified". Never advise scanning it "to see".
- Phone numbers in the message (callback phishing): check the number against the brand's official site (independently reached) and the official scam-check service; a number that appears only in the message is unverified by definition.

### Step 3 — Content and social-engineering analysis

- **What does it want?** Classify the ask: click, enter credentials/OTP, approve/consent, enter a device code, pay (especially to new/changed bank details), buy gift cards/crypto, call a number, install something, scan a QR, reply with personal data, keep a secret, act within a deadline.
- Pressure stack: urgency/deadline, threat (account closure, arrest, summons, fines, page deletion), secrecy ("don't tell your bank/family"), authority costume, flattery/rapport-building (investment and romance scripts run long games — early messages can look harmless; judge the *trajectory*: rapport → platform → returns → withdrawal fees).
- BEC/payment-redirection pattern: a known vendor/person suddenly supplying new bank details, a changed invoice, or a "confidential" payment instruction. Any payment-details change is verified by voice call to a previously known number, never by replying to the thread.
- Consistency: names, reference/case numbers, amounts, dates vs what is independently known (the user's mailbox history, contacts, and records, where you have access). A lure that names a *real* case or transaction is more dangerous, not less — scammers mine real context; verification still goes through independent channels.
- Language signals are weak evidence now: AI-written lures are fluent, and genuine senders make typos. Weight structure (authentication, destination, ask) over prose quality. Still note: generic greeting where the real sender would personalise, register mismatches, recycled template phrases seen in advisories.
- Too-good test: refunds, prizes, grants, returns — IRAS refunds are credited automatically to the registered bank/PayNow account; IRAS never asks for card/banking details by email to "process" one (SPF advisory, 29 Jan 2026).

### Step 4 — Attachments (metadata only; never open)

- High-risk types: executables and scripts (`.exe .scr .js .vbs .hta .lnk .iso .img`), macro-enabled Office files from unestablished senders, HTML attachments (credential-harvest pages), SVG with script, archives — above all **password-protected archives where the email itself supplies the password** (an AV-evasion signature).
- Double extensions (`invoice.pdf.exe`), extension/content mismatch, an attachment whose name tells a different story from the body.
- If a hash can be obtained without opening (attachment metadata from the provider), a hash-only reputation lookup is allowed; downloading to open or execute is not.

### Step 5 — External reputation and database checks

Run what your host can actually reach; log each check as run / not-run in the verdict. Several official services offer human-facing checkers but **no public API** — ScamShield is one: if you cannot query a service programmatically, do not report it as checked; say so, and route the user to run that check themselves.

- **ScamShield** (scamshield.gov.sg / app; helpline **1799**): Singapore's own database for links, numbers and messages; first stop for local lures — subject to the access honesty rule above. Use your jurisdiction's equivalent official checker where one exists.
- **Singapore Police Force advisories** (police.gov.sg): search the impersonated brand + pattern; SPF publishes variants with example sender addresses (e.g. IRAS phishing advisory 29 Jan 2026; SPF/TP/Courts iMessage advisory 8 Aug 2026; courier iMessage advisories Jul–Aug 2026).
- **CSA** (Cyber Security Agency of Singapore) alerts; **IRAS scam advisory** (go.gov.sg/iras-scam) for tax lures; **SMS SenderID registry** (smsregistry.sg) to verify a claimed SMS sender ID.
- **URL/domain reputation:** Google Safe Browsing lookup, VirusTotal domain/URL report (public web), urlscan.io search, PhishTank, Spamhaus domain check. A listing in any one of them as malicious is hard evidence.
- **Domain age/context:** WHOIS via search — a brand-lookalike domain registered days ago is near-conclusive; a "company" with no footprint outside the message is a flag.
- **Brand cross-check:** the real organisation's official site (independently reached) for scam alerts and its genuine contact points.

### Step 6 — Threat-intel refresh (self-updating pass)

Purpose: the indicator file must not freeze at build date. Sources, in priority order:

1. The user's jurisdiction first — the seeded set is Singapore: SPF / Anti-Scam Centre advisories and ScamShield updates (local patterns change fastest and matter most).
2. CSA alerts and advisories (or the user's national cyber agency).
3. Impersonated brands' own scam pages when a new brand appears in the user's traffic.
4. CISA / NCSC phishing guidance; APWG and major vendor trend reports (Microsoft Digital Defense, Mandiant, ESET, Proofpoint) via search summaries.
5. Reputable security press (Krebs on Security, BleepingComputer) for named novel techniques.

Method:
- Search each source for material **since the last refresh date**; extract only *new* techniques, indicators, impersonated brands and hard rules.
- **Append** dated, sourced entries to `references/indicators-and-techniques.md` (technique changelog + the relevant indicator section). Update the **Last refreshed** line. **Never delete or rewrite old entries** — mark superseded ones `[superseded — see <date entry>]`, so the file keeps a complete, auditable history.
- Fold in local experience: any message the user confirms was a scam (or a false alarm) becomes a dated, anonymised pattern entry — no personal data, no message bodies.
- A refresh is reported to the user in one short block: what was added, from which sources, and anything that changes how a pending verdict should be read.

## Scoring and verdict

Bands (report the band, the confidence, and the evidence):

- **CONFIRMED SCAM** — hard evidence: DMARC/auth failure on an impersonation; destination or sender listed malicious in a checked database; known advisory match (same pattern/sender as an SPF advisory); malicious-pattern attachment + matching lure.
- **LIKELY SCAM** — strong structural evidence short of a database hit: authority/bank impersonation from a non-anchor or lookalike domain; credential/OTP/payment ask landing on a non-official destination; payment-redirection from a lookalike or hijacked-looking thread.
- **SUSPICIOUS — verify independently before acting** — mixed or incomplete evidence; some checks could not be run.
- **LIKELY GENUINE** — authentication aligned, anchor domain, destinations match the text and the organisation, ask is routine and consistent with prior dealings; still list residual risks.
- **UNVERIFIED** — content-only screening (pasted message, screenshot) where identity checks were impossible.

Hard rules (floor verdicts, any channel):
- Government, court, police or bank impersonation from a free-mail, lookalike or non-anchor domain → at least **LIKELY SCAM**.
- A credential, OTP, device-code or consent ask whose destination/grant does not match the claimed organisation → at least **LIKELY SCAM**.
- A payment request to new/changed bank details arriving by message alone → **SUSPICIOUS at minimum**, and the verdict must instruct out-of-band voice verification regardless of how genuine it looks.
- WhatsApp: a request to share the 6-digit verification code, approve a linked-device request, or scan a WhatsApp pairing QR → at least **LIKELY SCAM** (account takeover in progress).
- WhatsApp: an install-this-APK instruction, or a work-document file (`.iso .zip .rar .exe`, scripts) sent with an instruction to open it on a computer — including when the sender is a known contact (compromised accounts, SPF 2 Nov 2026) → at least **LIKELY SCAM**; the verdict must instruct voice verification with the contact on a previously saved number before anything is opened or sent.
- Social layer: a message or conversation arc matching a social-scam category (romance, pig-butchering, impersonation/emergency, authority play, job/task, windfall, sextortion, marketplace play) is **earmarked and warned immediately** on the match, with a floor of **SUSPICIOUS**; if the matched play includes an ask for money, codes, installs or secrecy, the floor rises to **LIKELY SCAM**. Romance-related approaches from strangers or new contacts are earmarked on sight, before any ask appears.

Output format: follow `references/verdict-format.md` — verdict first, evidence grouped by sender / links / content / attachments / external checks (each item observed or inferred), what could not be checked, and what to do.

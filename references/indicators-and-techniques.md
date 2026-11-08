# Indicators and Techniques — living file for the Scam Detector skill

**Last refreshed: 2026-11-05 (seed build)**
Seeded 5 Nov 2026 from public sources, SPF advisories and 2026 trend reporting current at build. The skill's Step 6 refresh pass appends new dated, sourced entries and updates the line above. Entries are never deleted; superseded entries are marked `[superseded — see <date>]`.

---

## 1. Known-good anchors (Singapore — seeded set; extend for your own jurisdiction)

- **IRAS**: genuine mail from `iras.gov.sg`; corporate-tax alerts arrive via Postman from `info@mail.postman.gov.sg`. IRAS **never** asks for banking credentials or card details by email/SMS to process a refund; refunds are credited automatically to the bank account registered with IRAS or PayNow linked to NRIC/FIN/UEN, never to credit/debit cards (SPF advisory, 29 Jan 2026). IRAS scam advisory page: go.gov.sg/iras-scam.
- **Courts / judiciary**: correspondence from `judiciary.gov.sg` (e.g. FJC_FAMILY_REGISTRY@judiciary.gov.sg); eLitigation notices from `spadmin@elitigation.com.sg`. SPF, Traffic Police and the Courts **never** request payment via unsolicited links or message replies (SPF advisory, 8 Aug 2026). Verify any summons/offence notice at courts.gov.sg or police.gov.sg, or by official phone numbers from those sites.
- **Postman (gov.sg mail platform)**: legitimate government email sent via Postman comes from `.gov.sg`-aligned addresses; Postman itself never asks recipients for credentials, OTPs or payment.
- **Banks (SG)**: never email/SMS a login link; never ask for OTP, full card number or digital-token approval to "verify", "unlock" or "refund". Any message doing so is a scam regardless of branding.
- **Meta / Facebook**: never contacts users via Messenger with "your page/account will be deleted, verify now" — those are credential-harvest lures, always.
- **ScamShield**: check links/numbers/messages at scamshield.gov.sg or in the app; 24/7 helpline **1799**. SMS sender IDs can be verified at smsregistry.sg.

## 2. Email indicators

**Sender/authentication**
- SPF/DKIM/DMARC fail, or pass-but-misaligned with the visible From domain.
- Authority or brand display name on a free-mail (Gmail/Yahoo/Outlook.com) or random alphanumeric address.
- Lookalike domains: 1–2 character edits, homoglyphs, punycode `xn--`, brand-as-subdomain of an unrelated host, brand in the local part, brand with a wrong/cheap TLD.
- Reply-To or Return-Path on an unrelated domain. Thread suddenly pivoting to payment or links after a genuine history (thread hijacking).

**Links**
- Visible text ≠ href destination. Shorteners. IP-literal hosts. `@` in URL. Login pages on website-builder/file-sharing domains. Deep subdomain chains. Links present only in the HTML part.
- Consent/device-code lures on genuine login pages (see changelog: OAuth consent phishing, device-code phishing, ConsentFix).

**Content**
- The ask is the strongest content signal: credentials, OTP, device code, app consent, payment to new details, gift cards/crypto, callback to a number printed in the message, secrecy, hard deadline.
- Refund/prize/grant hooks; "minority who have not filed" style pressure is also used by *genuine* IRAS reminders — authority claims are decided by sender authentication and destination, not tone.
- AI-era note: fluent, personalised prose is no longer evidence of genuineness; sloppy prose is no longer proof of a scam. Structure over style.

**Attachments**
- `.exe .scr .js .vbs .hta .lnk .iso .img`; macro Office files from unestablished senders; HTML attachments; scripted SVG; double extensions; **password-protected archives with the password in the email body**.

## 3. Facebook / Messenger / Instagram / Threads indicators

- **Impersonation**: a "friend" or family member on a visibly new account; a celebrity/investor page with mismatched age/followers; support impersonation ("Meta support", page-deletion threats).
- **Pig-butchering / investment**: rapport-first conversations steering to a trading platform, showing paper profits, then withdrawal "taxes/fees". Move-off-platform pressure (to WhatsApp/Telegram) is a core tell.
- **Marketplace**: buyer overpayment ("I've paid extra, refund the difference"), fake payment screenshots, requests for email address to send a fake "payment confirmation", courier-redirection links, sellers demanding deposit to gift-card/crypto.
- **Account-recovery hijack**: "I sent a code to your number by mistake, forward it to me" — the code is the victim's own account reset code.
- **Giveaway/prize pages**: brand costume + "you've won" + a fee or card details to "claim".
- **Job scams**: easy-task commissions escalating to deposits.
- Any link in Messenger gets the same URL treatment as email (Step 2) — shortened and lookalike domains are the norm in these lures.
- **Account age:** Page Transparency creation date (Pages); "Joined Facebook" / Marketplace creation year (profiles); Instagram "About this account" Date joined + former usernames, where served. Young account + trust claim = flag; aged account + recent rename/rebrand = bought-or-repurposed flag — age is never a clearance on its own.

## 4. SMS / iMessage indicators

- SPF advisories (Jun–Sep 2026): courier-impersonation iMessages (DHL, NinjaVan, J&T, SingPost) from foreign numbers (+212, +63, +44…) or random email addresses; "parcel could not be delivered — invalid address"; update within hours via a lookalike link; tiny "re-delivery fee" S$0.06–S$1.64 to harvest card details; prompts to reply "Y"/"1" to defeat iMessage unknown-sender link protection. Couriers do not use iMessage for this.
- SPF/Traffic Police/Courts impersonation by iMessage: unpaid parking/traffic fines, unsettled summons, arrest-warrant threats, link/reply-code to a spoofed portal asking for card details, i-banking credentials or OTP (SPF advisory, 8 Aug 2026).
- SPF pop-up fine scams: browser "device locked" pop-ups with countdown timers demanding card payment — the Police cannot lock personal devices (SPF advisories, Feb & Sep 2026).
- Real SG SMS from organisations uses registered sender IDs (check smsregistry.sg); a long foreign number claiming to be a local authority or bank is a scam signature.

## 5. Technique changelog (dated, sourced — append only)

- **2026-11-05 (seed)** — AiTM (adversary-in-the-middle) phishing kits such as Tycoon2FA reverse-proxy real login pages and steal session tokens, defeating ordinary MFA; Tycoon2FA was disrupted in a multi-party operation announced 4 Mar 2026, but the kit class persists. Detection weight: a login link in an unsolicited message is hostile even if the page looks perfect; session-based compromise means "I have MFA" is not a clearance.
- **2026-11-05 (seed)** — OAuth consent phishing and **device-code phishing**: victim is sent to a *genuine* Microsoft/Google sign-in and induced to approve a malicious app or enter a device code; **ConsentFix** variant has the victim paste an OAuth-bearing URL back into a fake-CAPTCHA page. Detection weight: any message asking for app consent, a device code, or a paste-back is a hard-rule trigger.
- **2026-11-05 (seed)** — Quishing (QR-code phishing) is a top-growing vector (ESET telemetry: ~1 in 9 detected phishing emails in H1 2026 carried QR codes; Microsoft ranks QR the fastest-growing email vector). QRs move the victim to a phone, off protected endpoints, and hide the destination. PDFs carrying QR codes ("Scanception") recur.
- **2026-11-05 (seed)** — ClickFix / AI-fix: fake error/CAPTCHA pages instruct the victim to paste commands into their own terminal; AI-fix variants sit on legitimate AI-brand domains. Any message or page instructing "copy this command/fix" is a hard-rule trigger.
- **2026-11-05 (seed)** — Deepfake-assisted BEC: voice/video impersonation of executives and family members authorising transfers or pleading emergencies. Email-side tell is the payment/urgency ask; the countermeasure is out-of-band voice verification on a previously known number.
- **2026-11-05 (seed)** — MFA fatigue: repeated push approvals until the victim approves one. Messages "explaining" an approval prompt the user did not initiate are hostile.
- **2026-11-05 (seed)** — HTML smuggling and scripted SVG attachments deliver payloads/credential pages without classic executable attachments; treated in the attachment indicators.
- **2026-11-05 (seed)** — Singapore waves (SPF advisories, 2026): fake IRAS refund emails harvesting Singpass/banking credentials (29 Jan 2026); SPF/TP/Courts iMessage impersonation (8 Aug 2026); courier iMessage wave, 251+ cases and >S$1.2m losses since 24 Jun 2026 (5 Aug 2026); SPF pop-up fine scams (24 Feb, 20 Sep 2026).
- **2026-11-05 (seed)** — Tax-season timing: IRAS CIT filing season (Nov) and refund periods draw impersonation lures that mimic genuine reminder cadence. Genuine IRAS reminders (e.g. the Oracle AI Health and Porcupine Group YA 2026 CIT reminders received Oct–Nov 2026) came from the anchor route (Postman) and asked only to file at myTax Portal — never for credentials or payment details. Mimics that add links-to-login or detail requests fail the anchor test.

- **2026-11-05 (WhatsApp module build)** — Malware delivered through WhatsApp from **compromised accounts of people the victim knows** (friends, colleagues, supervisors): attachments dressed as work documents — observed filenames "Invoice 2026-09-15.iso", "Statement of Account_54d3.zip", "Salary Revision.rar" — with instructions to open them on a computer (WhatsApp Desktop/Web route); payload logs keystrokes, captures screenshots, beacons to a C2 server; the victim's own WhatsApp account is then taken over and used to spread the files and to solicit money from contacts (SPF advisory, 2 Nov 2026; SingCERT advisory, 27 Oct 2026; wider regional campaign disclosed by Kaspersky, Jun 2026). Detection weight: "it came from a known contact" is NOT exculpatory — unexpected `.iso .zip .rar .exe` or script files via WhatsApp match this pattern.
- **2026-11-05 (WhatsApp module build)** — Lure-to-APK with a fake-official second act, targeting seniors: Facebook/TikTok ads for senior activities collect contact details; the victim is contacted on WhatsApp and told to download an APK "to view activities"; a second scammer then impersonates MinLaw or the Police Anti-Scam Centre, claims the device was compromised by the app, and instructs transfers or handover of valuables (SPF advisory, 18 Jun 2026; at least 8 cases and S$69,000 lost since 1 Apr 2026).
- **2026-11-05 (WhatsApp module build)** — Compromised **students'** WhatsApp accounts used to impersonate children to their parents: claims of selection for a youth exchange programme or AI course, payment demanded via PayNow QR code (SPF advisory, 28 Sep 2026; at least 8 cases since Sep 2026, at least S$18,000 lost). Detection weight: a child's account asking a parent for course/exchange fees by QR is a named advisory pattern.
- **2026-11-05 (WhatsApp module build)** — Impersonation of **senior government officials, royalty and celebrities** over WhatsApp/Telegram: investment transfers under pretexts (joining a royal family, gifts), invitations to virtual meetings "with the President and Ministers" (SPF advisory, 6 May 2026). Singapore officials never ask the public to install apps from unofficial stores, transfer money, or hand over IC details over chat/calls.
- **2026-11-05 (WhatsApp module build)** — WhatsApp **account takeover** wave: (a) verification-code theft — the attacker triggers a genuine 6-digit code to the victim's phone, then asks for it under a pretext ("sent to you by mistake", a "friend", "WhatsApp support"); (b) device-linking — tricking the victim into approving a linked-device request or scanning a pairing QR via fake voting/contest/support pages (WhatsApp now surfaces suspicious-linking warnings). Takeover converts one victim into a launchpad against their whole contact list (2026 trend reporting; Hong Kong cases +170% in the first seven months of 2026; Meta banned 6.8M+ scam-centre accounts in H1 2026).

- **2026-11-05 (social layer build)** — Social-scam detection layer added in the v1.0 build: classification by the *nature* of the message rather than technical signals, with an immediate earmark-and-warn rule. Romance-related approaches from strangers/new contacts are earmarked on sight. Pattern cards in §8. Basis: the dominant 2026 SG pattern set — romance and pig-butchering arcs (Meta's enforcement reporting describes the funnel: text/dating-app opener → social media → private messenger → payment/crypto platform; 6.8M+ scam-centre accounts banned on WhatsApp in H1 2026), family/friend emergency impersonation, authority plays run as pure social pressure, and job/task scams whose openers are now routinely AI-written with small genuine payments used as trust bait before the deposit ask (FTC, Apr 2026; 2026 trend reporting).

- **2026-11-05 (account-age build)** — Account-age and tenure checks added in the v1.0 build. Hard dates where platforms serve them: Facebook Page Transparency (exact Page creation date, name-change history), Facebook "Joined" month/year and the Marketplace creation year, Instagram "About this account" (Date joined, country, former usernames — larger-reach accounts), X "Joined" on every profile. Proxies where no date is served: oldest visible content, rename history, numeric-ID magnitude (Instagram/Telegram IDs climb over time), group join dates, and first-seen in the user's own connected history. WhatsApp serves no creation date (tenure proxies only; SG number recycling means number age ≠ account age). Weighting: young account + trust-demanding claim is a strong negative; an old account is never a clearance — aged accounts are bought, renamed and hijacked, and old date + recent persona change is itself a flag.

## 6. Local confirmed patterns (from the user's own traffic — append only)

_(Empty at seed. Confirmed scams and confirmed false alarms from the user's channels are added here as dated, anonymised patterns — no names, no message bodies.)_

## 7. WhatsApp indicators (added 5 Nov 2026, WhatsApp module build)

**There is no authentication layer.** No SPF/DKIM/DMARC exists for WhatsApp; the only identity material is the phone number, the profile name/photo, and behaviour. Verdicts rest on number signals, trajectory, the ask, and advisory-pattern matching — and UNVERIFIED is the honest default for a bare forwarded message until a hard rule trips or a pattern matches.

**Number and account signals**
- WhatsApp exposes **no account creation date**. Tenure proxies only: saved-contact history, first-seen in the user's own connected records, group join dates. SG numbers are recycled — number age is not account age.
- A "friend", family member, bank, courier or agency writing from a number that is not the saved contact's number — above all the "this is my new number" opener — is impersonation until voice-verified on the previously saved number.
- Foreign country code presenting as a Singapore person or organisation (genuine SG contacts are +65); a local organisation running case, payment or verification business over a personal WhatsApp account.
- Unverified business profile claiming to be a bank, agency or brand; profile photo/name that is a celebrity, senior official or brand logo (SPF advisory, 6 May 2026 pattern).
- Being added by a stranger to an investment, job or "VIP" group; admins who prohibit voice calls or meeting; "mentor/teacher" personas posting staged profits.

**Compromised-account signals (the known-contact trap)**
- A genuine contact suddenly sending files, links or money requests out of character, in a changed writing style, at odd hours, or refusing a voice call. Since the Nov 2026 SPF wave, a real contact's account is a documented attack vector — familiarity of the account is not evidence the person sent it. Countermeasure: voice call to the previously saved number, or a question the real person would answer in context, before acting.

**The ask — WhatsApp-specific forms**
- Share the **6-digit verification code** ("sent to you by mistake") — account takeover; WhatsApp never asks for the code in a chat.
- Approve a **linked-device** request or scan a **pairing QR** (fake voting/contest/support pages) — account takeover by the second route.
- **Install an APK** or a "viewing/booking app" from a link or file — malware (SPF 18 Jun 2026 pattern); legitimate apps come from the official stores only.
- **Open a document on a computer** — `.iso .zip .rar .exe` or script files dressed as invoices/statements/salary files, including "forward it to your finance manager" (SPF 2 Nov 2026 pattern).
- **PayNow QR payment to an individual** for course/exchange fees, deposits, "unlock" fees or taxes on winnings/earnings — including from a child's own (compromised) account (SPF 28 Sep 2026 pattern).
- Job/task sequences: small genuine payments first, then a deposit, "upgrade" or crypto transfer to unlock earnings — if you must pay to be paid, it is a scam.
- **Move to Telegram** (or another app) to continue — the cross-platform funnel Meta's enforcement reporting describes (text/dating app → social → private messenger → payment/crypto platform); a pivot is a flag, not a preference.
- Voice notes and video calls pleading urgency: AI voice cloning and deepfake official/family calls are in active use — a familiar voice or face on WhatsApp is not identity proof. Callback on the previously saved number is the test.

**Handling rules for WhatsApp inputs**
- Screenshots: read the number, profile name/photo, timestamps and "forwarded / forwarded many times" labels as evidence; note that screenshots can be cropped or fabricated — weight the ask and the number over the story.
- Chat exports (.txt): the full thread gives trajectory (rapport → platform → returns → fees) that a single message hides; screen the whole arc, not just the last message.
- ScamShield checks work for WhatsApp numbers and links exactly as for SMS — run them (Step 5).

## 8. Social-scam pattern cards (added 5 Nov 2026, social layer build)

One card per play: the opener, the tells, the closing ask, and the counter. The social layer in SKILL.md carries the earmark-and-warn rule and the trajectory stages; these cards are the quick-match reference.

- **Romance / love scam.** Opener: a stranger's affectionate approach on a dating app, social media or messenger, or a "wrong number" that turns warm. Tells: love-bombing and destiny language within days; persona that cannot meet (deployed, offshore, widowed, travelling); modelled/stolen-looking photos; live video calls always failing; deep interest in the victim's finances framed as care. Closing ask: customs/medical/ticket money, a business emergency, or a shared "investment opportunity". Counter: the earmark fires at the approach; any money pivot confirms the play — verify by live video and by telling someone outside the conversation; never send.
- **Pig-butchering.** Opener: accidental contact, group invitation, or a romance/friendship that turns financial. Tells: casual wealth displays; a proprietary-looking trading app/site; an encouraged small withdrawal that succeeds; balances that only grow; pressure framed as mentorship. Closing ask: larger deposits, then withdrawal "taxes", "verification fees", "margin calls". Counter: independent check of the platform's licence and domain age; money already "in" the platform is treated as gone.
- **Family/friend emergency.** Opener: "new number", a child/relative in trouble, a friend who "lost their phone". Tells: secrecy from other family, urgency, refusal of voice calls ("can't talk"), payment to a third party's account or by QR. Counter: voice call to the previously saved number; a family code word where one exists; call the other parent/relative the secrecy was aimed at.
- **Authority play (social form).** Opener: an "officer" by call or message with a case, a warrant or a frozen account. Tells: stay-on-the-line pressure, agency hand-offs, secrecy from bank staff and family, "assistance" framed as transfers, valuables handover or app installs. Counter: hang up and dial the agency's published number; Singapore officials never run investigations this way (SPF advisories, 2026).
- **Job / task scam.** Opener: a recruiter's message offering flexible easy work. Tells: microwork with commission screenshots; a small real payment early; a dashboard of "earnings". Closing ask: deposits, upgrades or crypto to unlock higher tiers or withdraw. Counter: pay-to-be-paid is dispositive; verify the employer entirely outside the chat.
- **Windfall.** Opener: a prize, grant, inheritance or refund notice. Tells: a release fee, tax or card details demanded first; countdown pressure. Counter: genuine windfalls deduct fees at source; release-fee-first is the scam signature.
- **Sextortion.** Opener: a threat holding intimate images (real, scraped or fabricated). Tells: a short deadline, a payment demand, threats to send to contacts. Counter: do not pay (payment escalates demands), preserve evidence, report via ScamShield/Police; warn immediately — this card leads with the guidance, not the analysis.
- **Marketplace play.** Opener: a buyer/seller who moves fast and off-platform. Tells: overpayment plus "refund the difference", payment screenshots instead of cleared funds, courier-collection links, gift-card/crypto deposits. Counter: funds are real only when visible in the account itself, never in a screenshot or email.

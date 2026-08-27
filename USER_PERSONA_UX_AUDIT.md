# CyberSuraksha — Real-User UX Audit

Date: 27 August 2026  
Audited artifact: `index.html` served locally at `http://localhost:8000/index.html`  
Scope: rendered UI first, source-code comparison second. No application code was modified.

## 1. Executive summary

CyberSuraksha has a strong visual premise: the homepage immediately acknowledges urgency, prominently presents the Indian cyber helpline 1930, offers plain-language entry, supports Hindi, and exposes four understandable starting situations. The report flow is structured, includes evidence upload, jurisdiction selection, review, tracking, and a printable acknowledgement concept.

However, this is still a demo/prototype rather than a safely deployable cybercrime service. The largest risks are:

- The report form retains description, suspect fields, date, and evidence state when a user changes persona/category. A hacked-account or blackmail victim can therefore see another victim's financial narrative and may submit the wrong incident type.
- The UI claims secure routing, state Cyber Cell routing, anonymous reporting, official/I4C/RBI alignment, and complaint tracking, but the local implementation is primarily a browser `localStorage`/in-memory simulation. There is no verified backend/API submission in this environment.
- The final legal declaration is pre-checked, while the user is asked to affirm a statutory statement. This undermines informed consent and creates legal/trust risk.
- Instagram takeover and morphed/deepfake abuse are not first-class categories. The user must map them into broad “Hacked Account” or “Threats/Blackmail” buckets.
- Immediate recovery guidance is incomplete for the persona: Instagram account recovery, securing email, revoking sessions, warning contacts, platform takedown/reporting, and intimate-image safety are not integrated into the relevant report flow.
- English/Hindi coverage is partial: placeholders, some labels, demo shortcuts, footer text, and several generated fields remain English after Hindi is selected.
- Mobile presentation is usable but crowded: the fixed emergency bar covers content, the header clips at narrow width, and the most important reporting controls are below the fold.

Overall prototype score: **5.3/10**. It is promising for a guided demo, but I would not ask a distressed citizen to rely on it for a real complaint until the data, recovery, privacy, and backend claims are resolved.

## 2. Project overview

### What was found

- One-file, dependency-free HTML/CSS/JavaScript single-page application.
- Screens are shown/hidden with `showScreen()` rather than URL routes.
- Main screens: Home, Check a Scam, Safety Tips, Check Suspect, Track, report flow, confirmation, RBI letter tool, and Officials View demo.
- Report flow has four visible steps: Category → Incident → Details → Review.
- The homepage offers an AI-like typed/voice intake and scenario shortcuts.
- Hindi text is implemented through `data-en`/`data-hi` attributes; font scaling, high contrast, read-aloud, and an accessibility announcer are present.
- Storage abstraction uses `window.storage` if present, otherwise localStorage and memory fallback.

### Important implementation distinction

The UI presents itself as if it routes complaints to police/state cyber cells. In the local run, the code generates a random `NCRP2026######` reference and stores complaint/suspect records through browser storage. That proves a frontend simulation only; it does not verify a real NCRP/I4C, police, bank, SMS/OTP, GPS, or complaint-tracking integration.

## 3. Testing methodology

I started a local static server and used the rendered page as a first-time user. I inspected the homepage, navigated each primary item, opened the money, hacked-account, and threats/blackmail report paths, used the built-in sample data, advanced to identity/location/review screens, tested suspect lookup, toggled Hindi, opened the mobile layout at 390×844, and inspected the page screenshot. I deliberately did not submit a real-looking complaint, enter personal data, request an OTP, upload a private file, or invoke GPS.

I then compared observed behavior with the implementation in `index.html`, especially `flowState`, `startFlow`, `applySituation`, validation, storage, `submitReport`, localization, and responsive CSS.

Severity meanings: P0 = critical safety/completion risk; P1 = high; P2 = medium; P3 = polish.

## 4. Persona 1 — Indian bank/UPI fraud victim

Scenario: “I lost ₹50,000 through a UPI scam and need help immediately.”

### Journey map

| Stage | User goal | What user sees | Reaction/problem | Severity | Recommendation |
|---|---|---|---|---|---|
| Landing | Act quickly | Red “Lost money recently?” callout and 1930 CTA | Strong urgency and correct first action; useful for panic | Good | Keep the callout above the fold and add “also contact your bank/payment app now.” |
| Discovery | Find report | “I've lost money” card and AI intake | Very discoverable; “File a Report” is clear | P2 | Make the card a real keyboard-focusable button/link, not only a clickable `div`. |
| Incident | Explain fraud | Narrative, date, delay reason, optional suspect fields, evidence | Prompt is helpful, but asks the user to know UPI/UTR and does not show where to find them | P1 | Add examples/screenshots and a simple “I don’t have this information” path. |
| Evidence | Preserve proof | Optional PNG/JPG/PDF upload, 10MB limit | No checklist for bank statement, UTR, SMS, call log, recipient UPI, or screenshots; optional may imply evidence is unnecessary | P1 | Provide a short evidence checklist and safe-redaction guidance. |
| Details | Identify and route | Name, 10-digit mobile, OTP, state/district, police station | Requires identity and a simulated OTP; location defaulting to Delhi is dangerous | P0 | Never silently default jurisdiction; ask state/city clearly and disclose why it is needed. |
| Review | Confirm | Summary, legal declaration already checked, submit | Summary is useful, but statutory affirmation is pre-consented | P0 | Default unchecked; explain what is being affirmed in plain language. |
| Submit/track | Get confidence | Random NCRP reference and confirmation concept; Track asks for reference | In local run backend cannot be verified; no final real submission tested | P0 | Label demo mode visibly and connect tracking to an authenticated server before launch. |

### What worked

- 1930 is repeatedly visible and the message explains that speed matters.
- “UPI, Netbanking, OTP fraud…” is plain enough for discovery.
- The flow is short and visibly staged.
- Sample UPI content demonstrates useful fields such as phone number, UPI ID, amount, and UTR.
- Review includes location, station, suspect data, evidence, and complainant details.

### Moments of failure

- A distressed user is not explicitly told to call their bank/payment provider, block cards, freeze netbanking, change credentials, or preserve the transaction SMS before starting.
- The form hint assumes “UTR” and “UPI handle” knowledge without inline definitions.
- There is no amount-specific or transaction-by-transaction structure, so a user with multiple transfers must describe everything in one textarea.
- “Anonymous reporting” is not available for this category, but the reason and privacy implications are not explained.
- The UI uses “securely routed” language without a visible privacy/data-retention explanation.

### Score: 6.2/10

Discoverability 8, clarity 6, ease 6, flow 7, evidence guidance 4, trust 5, error prevention 5, emotional safety 6, mobile 6, backend confidence 2. The urgent CTA and compact flow are good; misleading production-like claims, Delhi defaults, and weak evidence/privacy guidance are serious for a high-stakes loss.

## 5. Persona 2 — Instagram account hacked

Scenario: “My password and email were changed; the attacker may be messaging my contacts.”

### Journey map

| Stage | User goal | What user sees | Reaction/problem | Severity | Recommendation |
|---|---|---|---|---|---|
| Landing | Know whether this qualifies | “Hacked account or phone” card | Understandable, but not Instagram-specific | P1 | Add “Instagram/Facebook/WhatsApp/email hacked” as explicit choices. |
| Discovery | Start safely | Hacked Account or Device screen | The screen inherits previously entered financial sample data during the same session | P0 | Reset all incident state on every new flow/category. |
| Immediate action | Recover account | Generic report form | No Instagram recovery link, hacked-account checklist, session/email security steps, or contact warning | P0 | Put platform recovery before police reporting, with a separate cybercrime escalation path. |
| Evidence | Capture proof | Generic optional upload and suspect URL field | No guidance for profile URL, username, changed-email alert, login notification, messages, or friends contacted | P1 | Add an evidence checklist and dedicated username/profile URL fields. |
| Submit | Report | Same identity/OTP/jurisdiction process | The user may not know whether platform recovery or this portal is the right next step | P1 | Explain scope: platform recovery/takedown plus police complaint when identity theft, extortion, fraud, or threats are involved. |
| Track | Follow case | Reference-number tracker | Backend and actual case status are unverified locally | P0 | Provide a real server-backed status model and explain response times. |

### What worked

- The label includes Instagram in the homepage description.
- The report can accept a social-media URL in the suspect field.
- The safety guide includes account-security concepts and 2FA.

### Moments of failure

- The same generic narrative hint says to include amount/UPI information, which is irrelevant or stressful for an account takeover.
- “Hacked Account or Device” collapses different problems: account recovery, malware, SIM swap, and identity theft.
- The quick-fill button says “WhatsApp Takeover,” not Instagram, and was observed to retain the previous UPI narrative after switching categories.
- No instructions say to secure the linked email first, revoke sessions, change passwords from a safe device, enable 2FA, warn contacts, or report impersonating messages.

### Score: 4.6/10

Discoverability 6, clarity 4, ease 5, flow 5, evidence guidance 3, trust 5, error prevention 2, emotional safety 4, mobile 6, problem effectiveness 3. The category is findable but the flow is not tailored to account takeover and can carry incorrect financial content into the report.

## 6. Persona 3 — Morphed images/videos or non-consensual distribution

Scenario: “My photos were manipulated and circulated; I fear further spread and blackmail.”

### Journey map

| Stage | User goal | What user sees | Reaction/problem | Severity | Recommendation |
|---|---|---|---|---|---|
| Landing | Feel safe and find help | “Threats or blackmail” with “Anonymous reporting available” | Relevant but frames the issue as blackmail; non-consensual image abuse and impersonation are less visible | P1 | Use a trauma-informed “Images/videos shared or altered without consent” entry. |
| Safety | Stop escalation | Safety guide has “Video Blackmail & AI Morphed Photos,” do not pay, and anonymous-path language | Good basic warning; no platform takedown/report steps or immediate safety plan | P1 | Add platform reporting, account privacy, contact-preservation, and threat-response guidance. |
| Discovery | Report without oversharing | Broad threat/blackmail form | The textarea encourages a detailed description; sensitive-content minimization is not explained | P0 | Ask only what is needed, allow “prefer not to describe,” and explain who can access the material. |
| Evidence | Preserve proof | Generic screenshots/PDF upload | No warning not to forward intimate material, no URL/account/post field tailored to platform, no content redaction guidance | P0 | Give safe evidence instructions and collect URLs/handles separately from the narrative. |
| Identity | Stay protected | Anonymous checkbox appears for this category | Helpful, but “anonymous” is not defined against logs, device data, or police follow-up; the final slip uses legal language | P0 | State exactly what anonymity means and its limits before selection. |
| Submit/track | Know what happens | Generic review and tracking | No trauma-informed confirmation, takedown expectation, safety contact, or escalation route | P1 | Confirm receipt without repeating sensitive content and provide support/escalation options. |

### What worked

- Homepage copy explicitly mentions morphed photos/video blackmail.
- Safety content says not to pay ransom and recognizes AI face-swapped media.
- An anonymous route is visually offered for threats/blackmail.
- The interface does not require uploading a file; upload is optional.

### Moments of failure

- “Anonymous reporting available” is a strong promise, but the implementation only suppresses name/mobile in the complaint object; it does not explain metadata, logs, storage, or the possibility of follow-up.
- The legal/sensitive terminology (“sextortion,” IT Act sections, “Sec 67 IT Act”) may increase fear and is not translated consistently.
- The form is still generic and carries financial form hints/state if prior data exists.
- No distinction between consensual adult content, minor safety, impersonation, blackmail, harassment, or defamation; these cases need different safeguarding.

### Score: 4.1/10

Discoverability 6, clarity 4, ease 4, flow 4, evidence guidance 2, trust 4, privacy 3, emotional safety 3, mobile 6, effectiveness 3. The promise of anonymity and recognition of morphed media are positive, but privacy boundaries and victim-safe handling are inadequate.

## 7. Persona 4 — Curious visitor

### Journey map

| Stage | User goal | What user sees | Reaction/problem | Severity | Recommendation |
|---|---|---|---|---|---|
| Landing | Understand site | “Report cybercrime simply…” plus 1930 | Clear purpose and urgency; official status is ambiguous because footer says concept redesign/not government endorsed | P1 | Put “prototype/demo” or official ownership in one prominent trust statement. |
| Explore | Learn options | Four situation cards, Check a Scam, Safety Tips, Check Suspect, Track | Good information architecture at a glance | Good | Preserve this top-level structure. |
| Trust | Know who operates it | Footer disclaimer, trust claims, I4C/MHA/RBI references | Mixed signals: government-like visual language but explicit non-affiliation | P1 | Explain operator, data controller, security, and relationship to official portals. |
| Learn | Prevent harm | Long interactive guide and self-audit | Rich content, but many absolute statistics/legal claims feel authoritative without sources | P1 | Cite official sources and date claims; replace unsupported percentages with plain guidance. |
| Act | Report/check/track | Demo shortcuts and tools | “Officials Queue” and demo shortcuts can confuse a normal visitor about what is real | P2 | Put demo affordances behind a clearly labeled prototype mode. |

### Score: 6.3/10

First impression 8, discoverability 8, clarity 7, navigation 7, trust 4, accessibility 6, mobile 6, content usefulness 7. The landing experience is compelling and approachable, but trust authenticity is the decisive weakness.

## 8. Friction-point analysis

### Critical blockers

- Cross-persona form-state leakage can cause wrong-category or wrong-evidence submission.
- No verified backend, OTP service, police routing, or persistent case tracking in the local environment.
- Default Delhi/South Delhi jurisdiction can misroute a user who does not notice it.
- Pre-checked statutory declaration creates an uninformed-consent/legal risk.
- Sensitive-content flow does not sufficiently protect victims from oversharing or explain anonymity limits.

### Major friction

- Generic form wording across financial, hacked-account, and abuse cases.
- No first-class Instagram takeover or non-consensual media workflow.
- Weak evidence guidance and no “where to find UTR/profile URL” help.
- Missing bank/payment-provider and social-platform recovery actions in the main flow.
- Official-looking claims conflict with the disclaimer and demo behavior.
- Hindi does not cover all user-facing strings/placeholders.

### Minor friction

- Demo quick-fill chips remain visible to normal users and use artificial sample identities.
- “Suspect Telemetry” is intimidating technical language for a citizen review.
- Long Safety Tips page has high cognitive load and repeated official-sounding labels.
- Mobile fixed bar competes with content and header controls are crowded.

### Good experiences

- Clear 1930 emergency call-to-action.
- Plain-language homepage categories.
- Visible four-step progress model.
- Review screen offers edit actions.
- Hindi toggle, font-size controls, high contrast toggle, read-aloud, and live announcer show good accessibility intent.
- Suspect lookup gives immediate feedback and warns against paying/sharing OTPs.

## 9. Accessibility audit

### Positives

- Semantic headings, labeled primary navigation, labeled textboxes/buttons, checkbox labels, `aria-live` announcer, visible focus styling, language attribute switching, font scaling, high-contrast mode, and keyboard-oriented controls are present.
- OTP inputs use numeric input mode.
- Buttons have generally generous padding and the emergency action is prominent.

### Risks and likely violations

- Situation cards and diagnostic cards are clickable `div`s without button/link semantics, keyboard activation, or guaranteed focus indication.
- Hidden screens use `aria-hidden`, but screen-management focus and hidden content should be tested with a real screen reader; the active heading gets `tabindex=-1`, which is a good start but not sufficient proof.
- The dropzone is a clickable `div`; the actual file input is `display:none`, so keyboard users may not be able to reach upload naturally.
- Some form labels are plain text adjacent to inputs/selects rather than consistently associated `<label for>` elements.
- The textarea’s placeholder is not translated when Hindi is selected.
- Mobile menu state is visually controlled but its expanded/collapsed state is not exposed with `aria-expanded`.
- Contrast and color meaning need automated and manual WCAG testing, especially status chips, muted text, red emergency text, and high-contrast overrides.
- No visible reduced-motion preference despite smooth scrolling and transitions.
- Read-aloud depends on browser speech behavior and has no clear status/error feedback.

Accessibility score: **5.8/10**.

## 10. Mobile UX audit

Tested at 390×844. The page reflows into one column and the 1930 action is easy to find. The screenshot showed a crowded header with clipped right-side controls and a fixed bottom emergency bar overlaying the page near the AI intake card. The report form’s two-column date/timeline and location grids should be checked carefully on older Android widths.

Recommendations:

- Collapse header controls into a clearly labeled menu; expose `aria-expanded` and keep 1930 as the single persistent action.
- Make the bottom bar shorter and ensure it never overlays focused inputs, upload controls, or submit buttons.
- Put “Call 1930 first” and the main report CTA above the AI assistant on small screens.
- Test 320px width, large text, Hindi, landscape, slow network, and keyboard/switch access.
- Avoid loading remote fonts as a requirement for usable first paint on mobile data.

Mobile score: **6.0/10**.

## 11. Indian-context audit

### Language

Hindi is a meaningful improvement, but the experience is not fully bilingual. English-only placeholders, “Demo shortcuts,” some footer content, and technical/generated labels remain. There is no regional-language strategy beyond Hindi.

### Digital literacy

UPI and OTP are familiar to many users, but UTR, UPI handle, IFSC, APK, RAT, telemetry, NCRP, BNS, IT Act, and “shadow credit” need plain-language explanations. Replace “suspect telemetry” with “Details about the scammer” and provide examples beside fields.

### Device and connectivity

The static zero-dependency architecture is lightweight, but voice recognition, remote fonts, file upload, and mobile overlays need testing on low-end Android and poor networks. Users need a save/resume or printable offline checklist if reporting is interrupted.

### Emotional state

The 1930 message responds well to panic. The form itself remains bureaucratic and generic. Use calm, non-blaming wording, avoid unnecessary legal intimidation, and show immediate next steps after every category selection.

Indian-context score: **5.7/10**.

## 12. Trust and safety audit

The footer explicitly says the site is “a concept redesign for a UI/UX competition” and “not affiliated with or endorsed by the Government of India.” This is honest, but it conflicts with prominent phrases such as “reports routed to your state cyber cell,” “I4C & MHA Awareness Initiative,” “official citizen manuals,” “NCRP complaint lifecycle,” and “securely routed.” A real citizen could reasonably assume this is an official portal.

Before any real use, the service must publish:

- Operator and official ownership/partnership.
- Privacy notice, data retention, access controls, encryption, and deletion policy.
- Exact meaning and limits of anonymous/confidential reporting.
- Whether data is shared with police, banks, platforms, or other authorities.
- Expected response time and what the portal cannot do.
- Clear parallel actions: 1930, bank/payment app, telecom provider, Instagram/platform, and emergency/police support where appropriate.

Trust/safety score: **3.8/10**.

## 13. Persona scorecard

| Metric | Bank fraud | Instagram hack | Morphed content | Curious visitor |
|---|---:|---:|---:|---:|
| Discoverability | 8 | 6 | 6 | 8 |
| Clarity | 6 | 4 | 4 | 7 |
| Ease of use | 6 | 5 | 4 | 7 |
| Reporting flow | 7 | 5 | 4 | N/A |
| Evidence guidance | 4 | 3 | 2 | N/A |
| Trust | 5 | 5 | 4 | 4 |
| Mobile UX | 6 | 6 | 6 | 6 |
| Accessibility | 6 | 6 | 5 | 6 |
| Emotional safety | 6 | 4 | 3 | 6 |
| Overall | **6.2** | **4.6** | **4.1** | **6.3** |

Overall weighted prototype score: **5.3/10**.

## 14. Prioritized issue list

| ID | Priority | Persona | Page/component | Problem/evidence | User impact | Recommended fix | Complexity |
|---|---|---|---|---|---|---|---|
| P0-01 | P0 | All reporters | Report flow state | `flowState` and form inputs are not reset when `startFlow()`/`applySituation()` changes category; observed UPI narrative on hacked and blackmail screens | Wrong or leaked incident data can be submitted | Create a fresh per-case model, reset narrative/date/suspect/files/OTP/location state, and confirm category change | M |
| P0-02 | P0 | All | Storage/submit/track | `submitReport()` generates a random reference and writes browser storage; real backend/routing unverified | False confidence; complaint may not reach authorities | Integrate and test a real backend, or label every screen as demo-only and remove official claims | L |
| P0-03 | P0 | Bank fraud | Location | Location starts at Delhi/South Delhi; auto-detect function simulates Delhi rather than GPS | Misrouting and unsafe false assurance | Require explicit state/city confirmation; never simulate GPS in production | M |
| P0-04 | P0 | All | Review | `declaration-check` is checked by default | Informed legal affirmation is bypassed | Default unchecked, explain declaration, block submission until deliberate selection | S |
| P0-05 | P0 | Morphed content | Anonymous path | Anonymous means name/mobile omitted in stored object, but limits/metadata/access are not explained | Victim may disclose sensitive material believing they are untraceable | Publish a precise privacy model, minimize data, add safe-content handling and escalation | L |
| P1-01 | P1 | Instagram | Category/recovery | No Instagram-specific recovery or platform-reporting journey | Victim may lose account or allow attacker to harm contacts | Add recovery checklist, linked-email security, session revocation, contact warning, URL/handle fields | M |
| P1-02 | P1 | Morphed content | Evidence | Generic optional upload; no URL/takedown/redaction guidance | Evidence lost or sensitive files spread further | Add trauma-informed evidence checklist and separate URLs/handles from media upload | M |
| P1-03 | P1 | Bank fraud | Incident | Generic narrative asks for UTR/UPI without “where to find it” help | Abandonment or incomplete reports | Add transaction rows, examples, bank-app instructions, and “not available” choice | M |
| P1-04 | P1 | All | Trust | Government-like claims conflict with concept disclaimer | User hesitates or submits to an untrusted demo | Clarify ownership and production status in header and legal/privacy content | S/M |
| P1-05 | P1 | Hindi users | Localization | Placeholder, footer, demo and some generated labels remain English | Lower comprehension and trust | Centralize all strings; test every screen in Hindi and add regional-language plan | M |
| P1-06 | P1 | Accessibility | Cards/upload/menu | Clickable div cards, hidden file input, incomplete label associations, no `aria-expanded` | Keyboard/screen-reader users may be blocked | Use semantic controls, focus management, accessible file button, expanded state | M |
| P2-01 | P2 | All | Immediate actions | Bank, payment-app, Instagram, telecom and email actions are not integrated into report flow | Users may report but fail to contain harm | Add category-specific checklist before form and repeat on confirmation | M |
| P2-02 | P2 | Curious | Safety guide | Unsupported precise percentages/legal claims and official-sounding labels | Misleading authority and misinformation risk | Cite dated primary sources and soften unsupported claims | M |
| P2-03 | P2 | Mobile | Header/bottom bar | Narrow screenshot showed clipped header and emergency bar overlay | Missed controls and obscured content | Rework responsive header/bar and test large text | M |
| P2-04 | P2 | All | Form language | “Suspect Telemetry,” “RBI dispute,” “Sec 65B,” and similar jargon | Anxiety and misunderstanding | Plain-language labels with expandable definitions | S/M |
| P3-01 | P3 | Curious | Home | Demo shortcuts and Officials Queue visible beside citizen actions | Prototype behavior can be mistaken for production | Hide behind prototype mode or label strongly | S |
| P3-02 | P3 | All | Motion/speech | Smooth-scroll/read-aloud status and reduced-motion support are incomplete | Disorientation or inaccessible feedback | Respect `prefers-reduced-motion`; expose recording/read-aloud state | S/M |

## 15. What we are doing well / poorly

### Doing well

- Urgency is communicated without blaming victims.
- 1930 is prominent and actionable.
- Entry categories use ordinary language.
- Hindi, font scale, contrast, read-aloud, and live announcements demonstrate inclusive intent.
- The flow has a clear progress model and review/edit step.
- The app anticipates Indian scam patterns: UPI QR traps, digital arrest, APK scams, SIM swap, task scams, and morphed media.

### Doing poorly

- Treating a high-stakes reporting service as a convincing local simulation without equally prominent demo labeling.
- Reusing mutable form state across materially different victim journeys.
- Offering generic forms where victims need immediate, category-specific containment.
- Making evidence optional without teaching safe preservation.
- Overstating anonymity, official alignment, routing, and legal/RBI certainty.
- Leaving technical language and English strings in a supposedly plain-language Hindi experience.

## 16. Feature gaps

- Real complaint backend, authentication, OTP delivery, audit log, status updates, and authority routing.
- Bank/payment-provider escalation and 1930 handoff instructions.
- Platform-specific recovery/takedown for Instagram and other services.
- Structured UPI/card/bank transaction capture.
- Safe evidence vault with redaction, retention, access, and upload status.
- Trauma-informed image-abuse and minor-safety branching.
- Privacy policy and explicit anonymity/confidentiality model.
- Save-and-resume, offline fallback, network failure recovery, and duplicate-submission prevention.
- Government verification and official-source citations.

## 17. Recommended improvements

### P0: Make the prototype safe to understand

1. Replace production-like claims with a persistent “Prototype — no complaint is sent to authorities” notice until backend integration is real.
2. Reset form state on every new report and show a category-specific form model.
3. Uncheck the legal declaration by default and make location explicit.
4. Add a privacy/safety interstitial before sensitive-content reporting that explains data use, anonymity limits, and safe evidence handling.

### P1: Make each user journey actually useful

1. First question: “What happened?” with Bank/UPI Fraud, Account Hacked, Fake Profile/Impersonation, Images/Video Shared or Altered Without Consent, Threats/Blackmail, SIM/Phone, and Other.
2. After selection, show three immediate actions, a short evidence checklist, then the complaint form.
3. For bank fraud, capture transaction details with examples of UTR/transaction ID and a direct bank/1930 reminder.
4. For Instagram, provide Instagram recovery, linked email, sessions, 2FA, contact warning, profile URL, and platform-reporting steps.
5. For morphed content, minimize narrative burden, collect post/profile URLs separately, warn against forwarding intimate material, and provide platform takedown and safety escalation.

### P2/P3: Improve comprehension and reach

- Translate all strings and add language coverage testing.
- Replace clickable `div`s with semantic controls.
- Fix mobile header and fixed-bar overlap.
- Cite legal/statistical claims and add dated official links.
- Add clear error, offline, upload-progress, duplicate, and success states.

## 18. Ideal future user journeys

### Bank fraud

Homepage → “Money lost” → Call 1930 + contact bank/payment app → Choose UPI/card/bank/OTP → Enter transaction(s) with examples → Evidence checklist/upload → Identity and jurisdiction explanation → Review with unchecked declaration → Submit to verified backend → Confirmation with complaint ID, expected timeline, bank follow-up, and Track.

### Instagram hack

Homepage → “Account hacked” → Secure linked email and use Instagram recovery → Warn contacts and capture alerts/profile URL → Choose “Cybercrime complaint needed?” → Report identity theft/fraud/threats → Minimal evidence → Verified submission → Recovery/takedown and complaint tracking guidance.

### Morphed or non-consensual media

Homepage → “Images/videos shared or altered without consent” → Immediate safety and platform-reporting steps → Choose impersonation/blackmail/harassment/minor safety → Collect URLs/handles and safe screenshots without re-uploading intimate content unless required → Explain confidentiality/anonymity limits → Submit → Trauma-informed confirmation, takedown steps, and escalation contacts.

### Curious visitor

Homepage → What this service is / who operates it → What can be reported → Learn by category → Prevention tips with official sources → Report only when needed → Track with an explained reference number.

## 19. Prioritized roadmap

### Release gate — before any real citizen use

- Verified backend and authority integration.
- Privacy/security review and data-minimization design.
- State/jurisdiction correctness.
- Reset-state and submission integrity tests.
- Legal/content review of RBI, BNS, IT Act, I4C, and police claims.

### Next iteration

- Category-specific journeys and recovery actions.
- Evidence guidance and sensitive-content safety.
- Full Hindi string coverage.
- Semantic accessibility and mobile fixes.

### Later polish

- Save/resume, richer status updates, source-linked learning content, regional languages, and improved visual density.

## 20. Final answer by persona

### If I had just lost ₹50,000 to a UPI scam: **PARTIALLY**

I would quickly find 1930 and the financial-fraud report, but I would not confidently trust the jurisdiction, backend submission, or tracking. The form does not give enough immediate bank/payment containment guidance or evidence help, and a reused session can carry incorrect data.

### If my Instagram account had been hacked: **NO**

The broad hacked-account option is discoverable, but the experience does not guide Instagram recovery, linked-email security, contact warnings, platform reporting, or the distinction between account recovery and a police complaint. The observed state leakage makes this journey unsafe.

### If my photos/videos had been morphed and circulated: **NO**

The site recognizes morphed content and offers an anonymous-looking path, but it does not adequately explain confidentiality, minimize traumatic disclosure, protect sensitive evidence, or guide platform takedown and safety actions. A vulnerable victim could misunderstand the privacy promise.

### If I were only curious: **PARTIALLY**

I would understand the purpose and find the main tools, but I could mistake this polished concept for an official Government of India service. The footer disclaimer and official-sounding claims conflict, and demo functions are not consistently separated from real-service expectations.

**Bottom line:** the prototype has a strong citizen-friendly foundation, but a real Indian citizen should not yet be expected to complete a sensitive cybercrime report confidently without another person’s explanation.

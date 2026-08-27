# CyberSuraksha Competition Improvement Backlog

## Goal

Make CyberSuraksha easier and more useful for every Indian citizen, regardless of age, technical knowledge, language, device, or emotional state.

This backlog is intentionally focused on demo interactions, content, accessibility, and usefulness. Real backend integrations are not required for the next iteration.

## 1. Add a simple first question

Replace the current broad starting experience with:

> **What happened to you?**

Show large, plain-language choices:

- Money was taken from my bank/UPI/card.
- My Instagram, WhatsApp, email, or social account was hacked.
- Someone created a fake profile using my name or photo.
- My photo or video was changed or shared without permission.
- Someone is threatening, blackmailing, or harassing me.
- I received a suspicious call, message, link, or APK.
- My SIM/phone stopped working unexpectedly.
- I am not sure — help me decide.

Each option should show one short example and an icon.

## 2. Create a guided “Help me decide” mode

For users who do not know whether an incident is cybercrime:

1. Ask one question at a time.
2. Use Yes / No / Not sure buttons.
3. Avoid legal terminology.
4. Explain the selected result in plain language.
5. Offer “Get immediate help,” “Report,” and “Learn more.”

Example:

> Did someone ask you to send money, share an OTP, or scan a QR code?

## 3. Add an urgent-action panel before every report

Before opening the form, show three immediate actions specific to the incident.

### Financial fraud

- Call 1930 immediately.
- Contact the bank, wallet, or payment app.
- Block cards or freeze the account if necessary.
- Do not delete SMS, transaction messages, or chat evidence.

### Hacked account

- Secure the linked email first.
- Change the password from a safe device.
- Log out other sessions.
- Turn on two-factor authentication.
- Warn friends and family not to trust messages from the account.

### Morphed or non-consensual content

- Do not pay or negotiate with the blackmailer.
- Save the post/profile/message URL safely.
- Report the content directly on the platform.
- Do not forward intimate material to other people.
- Contact a trusted person or emergency support if there is an immediate threat.

### Malicious APK or phone compromise

- Disconnect mobile data/Wi-Fi if safe to do so.
- Contact the bank from another device.
- Remove suspicious apps only after preserving essential evidence.
- Change banking and email passwords from a safe device.

## 4. Make every report form category-specific

Do not use the same description hint and fields for every incident.

### Financial fraud fields

- Bank, wallet, or payment app.
- Type: UPI, card, net banking, wallet, investment, loan, job scam.
- Transaction date and time.
- Amount lost.
- Transaction ID/UTR.
- Receiver UPI ID/account, if known.
- Bank complaint number, if already reported.

### Hacked account fields

- Platform: Instagram, WhatsApp, Facebook, email, other.
- Username/profile URL.
- When access was lost.
- Whether email/phone was changed.
- Whether the attacker contacted others.
- Whether money, threats, or impersonation are involved.

### Morphed/deepfake content fields

- Content type: photo, video, profile, message, deepfake.
- Platform.
- Post/profile/message URL.
- Account username or handle.
- Is there a threat or blackmail request?
- Is a minor involved?
- Safe evidence upload with clear privacy warning.

### Suspicious message/link fields

- Phone number, email, UPI ID, website, APK, or social handle.
- How the user received it.
- Whether the user clicked, installed, shared information, or paid.

## 5. Add a clear evidence checklist

Cybercrime.gov.in does this better and CyberSuraksha should include a simpler version.

Show a checklist before upload:

- Incident date and time.
- Screenshots of messages, profiles, calls, or posts.
- Bank SMS or transaction receipt.
- 12-digit transaction ID/UTR for financial fraud.
- Phone number, email, UPI ID, URL, or social-media handle.
- Bank/payment-app complaint reference.
- Relevant documents, if requested.

Add “Where can I find this?” help beside each item.

Add “I do not have this yet” so users are not blocked.

## 6. Explain technical words inline

Use an information icon or “What does this mean?” link.

Terms to explain:

- UTR: The transaction reference number shown in your bank or UPI receipt.
- UPI ID: The address used to send or receive money.
- OTP: A one-time code sent to your phone.
- URL: The website or profile link shown at the top of the screen.
- Evidence: Information that helps show what happened.
- Complaint ID: The number used to check your report later.
- APK: An Android app file that may be unsafe if received from an unknown link.

## 7. Design for different age groups

### Children and teenagers

- Add “I am under 18” or “A child is involved.”
- Use simple language and larger buttons.
- Provide trusted-adult guidance.
- Avoid forcing the child to repeatedly describe sensitive content.
- Make emergency escalation visible.

### Older adults

- Add “Read this aloud.”
- Use larger default text and high-contrast buttons.
- Avoid abbreviations in headings.
- Use step-by-step instructions with one action per screen.
- Add “Ask a family member to help” without making the user feel blamed.
- Provide a printable/downloadable checklist.

### Low digital-literacy users

- Use examples instead of technical labels.
- Add image-based explanations of a UPI receipt, OTP message, and profile URL.
- Use “Money was taken” instead of only “Financial cyber fraud.”
- Provide voice input and read-aloud in Hindi.

### Experienced users

- Offer an “Advanced details” section for UTR, IP, URL, headers, account numbers, and additional evidence.
- Keep advanced fields optional and collapsed by default.

## 8. Improve language support

Include full Hindi coverage for:

- Navigation.
- Placeholders.
- Error messages.
- Evidence checklist.
- Confirmation and tracking.
- Safety instructions.
- Privacy content.

Plan future support for regional languages such as Tamil, Telugu, Bengali, Marathi, Kannada, Malayalam, Gujarati, and Punjabi.

Add a language choice during the first visit:

- English.
- हिंदी.
- Use my phone language.

Do not leave mixed English/Hindi text on important screens.

## 9. Add voice and assisted-use mode

Keep the current voice concept, but make it more useful:

- Say “Speak in Hindi or English.”
- Show the recognized text before using it.
- Let the user edit the transcript.
- Add “I prefer typing.”
- Provide read-aloud for every instruction and error.
- Explain that voice input may use browser speech services.
- Add large “Back,” “Next,” and “Repeat” controls.

## 10. Make the report flow visibly safer

Use this structure:

```text
1. What happened?
2. What should you do immediately?
3. Tell us the important details
4. Add evidence if available
5. Choose privacy/contact preference
6. Check your summary
7. See what happens next
```

Show:

- Current step.
- Number of steps remaining.
- Save draft for later.
- Back without losing information.
- Clear error messages beside the relevant field.
- A “Not sure” option wherever possible.

## 11. Add privacy choices in plain language

Before sensitive reports, explain:

- What information is being requested.
- Why it is needed.
- Who may see it.
- How long it is kept in the demo.
- Whether the user can report anonymously.
- What anonymous reporting can and cannot protect.

Use options such as:

- I want to report with my contact details.
- I want to report anonymously where available.
- I need help understanding the privacy options.

Do not pre-check legal declarations or consent checkboxes.

## 12. Add safer sensitive-content handling

For morphed images, intimate content, blackmail, and child safety:

- Use calm, non-judgmental language.
- Do not require a detailed traumatic description when unnecessary.
- Allow “I do not want to describe the content.”
- Ask for URLs and usernames separately.
- Warn users not to forward intimate files.
- Add a “content may involve a child” urgent path.
- Add platform-reporting guidance.
- Avoid repeating sensitive content in confirmation screens.

## 13. Improve the homepage

Keep the strong 1930 CTA, but add three clearly separated actions:

- **I need help now**
- **I want to report something**
- **I want to learn or check something**

Add a short “What this service can help with” section.

Add a visible “Not sure where to start?” button.

Move demo-only shortcuts into a clearly labeled “Prototype demo tools” area.

## 14. Add a better “What happens next?” explanation

After the review screen, show a simple timeline:

```text
Report submitted → Reference number created → Review by authority → Follow-up/investigation → Status update
```

Explain:

- What the user should save.
- When to contact the bank or platform again.
- How to use the complaint ID.
- What the portal cannot guarantee.
- What to do if there is immediate danger.

## 15. Improve tracking for the demo

Use realistic sample tracking states:

- Report received.
- Information reviewed.
- Assigned to relevant cyber cell.
- Bank/payment action requested.
- Investigation in progress.
- Additional information needed.
- Closed/resolved.

Add:

- Sample complaint ID button.
- Copy complaint ID.
- Download/print acknowledgement.
- “What this status means” explanation.
- Empty-state example for a new user.
- Invalid-ID error with recovery guidance.

## 16. Add current cybercrime.gov.in strengths

### Official identity and trust structure

Include a clearly labeled demo version of:

- Who operates the future service.
- Relationship to the National Cyber Crime Reporting Portal.
- Privacy policy.
- Disclaimer.
- Contact/help information.
- FAQ.
- Website policies.

For the competition demo, say clearly that these are proposed future production elements.

### Complaint preparation checklist

Add the official portal’s preparation concept, but make it friendlier and easier to understand.

### Legal expectation screen

Add a short, plain-language confirmation:

> “Please provide information that is true to the best of your knowledge. Authorities will review the complaint according to applicable law.”

Keep the full legal wording behind “Read the complete notice.”

### Anonymous reporting path

Keep anonymous reporting for relevant women/children and sensitive-content cases, but explain its limits.

### Financial-fraud separation

Give financial fraud its own fast route, with bank/wallet/merchant, transaction ID, date, amount, and evidence fields.

### Other cybercrime route

Keep a broad “Other cybercrime” option for incidents that do not fit the main cards.

### Suspect repository

Improve the current lookup by allowing searches for:

- Mobile number.
- Email ID.
- UPI/account identifier.
- Website URL.
- Social-media handle.
- APK/file name.

Add a clear warning:

> “A clean result does not prove that an identifier is safe.”

### Report a suspect

Add a separate simple flow to report:

- Phone number.
- WhatsApp/Telegram handle.
- Email.
- Website URL.
- Social-media URL.
- SMS sender/header.

### Learning Corner

Keep CyberSuraksha’s more approachable cards, but add official-style sections:

- Citizen manual.
- Safety tips.
- Cyber awareness.
- Scam-of-the-day/daily digest.
- Frequently asked questions.

### Help and support links

Add visible links for:

- FAQ.
- Contact/help.
- Feedback.
- Privacy.
- Disclaimer.
- 1930 cyber helpline.
- 112 emergency police help when appropriate.
- 181 women’s helpline when appropriate.

## 17. Improve accessibility interactions

- Convert clickable cards into real buttons or links.
- Make every control keyboard reachable.
- Add visible focus states.
- Use `aria-expanded` for the mobile menu.
- Associate every label with its input.
- Make upload usable without a mouse.
- Use inline error messages and `aria-live` announcements.
- Respect reduced-motion settings.
- Keep touch targets at least comfortably finger-sized.
- Test with screen reader, keyboard-only use, and large text.

## 18. Improve mobile usefulness

- Remove horizontal overflow at 360px and 390px.
- Keep the header inside the viewport.
- Make the emergency bar non-blocking.
- Use one-column forms on narrow screens.
- Keep the main action visible near the bottom of each step.
- Add a sticky “Back / Next” control only if it does not hide content.
- Test portrait, landscape, low bandwidth, and large-font modes.

## 19. Add demo states that make the product feel complete

Every major feature should show:

- Empty state.
- Loading state.
- Success state.
- Error state.
- Retry state.
- “Not available yet” state for simulated functionality.

Examples:

- Suspect identifier not found.
- Complaint ID invalid.
- File too large.
- Unsupported file type.
- Voice recognition unavailable.
- Demo storage unavailable.
- User has not prepared all evidence.

## 20. Improve demo credibility without heavy backend work

- Add a “Demo mode” label in the header.
- Use a sample complaint button rather than implying a real submission.
- Add a “Reset demo” control.
- Show a small “This screen is simulated” note on confirmation/tracking.
- Use consistent sample data across all pages.
- Add one presenter-friendly complete journey.
- Make Officials View clearly a prototype view.

## 21. Do not change these strong foundations

- Prominent 1930 emergency CTA.
- “What happened to you?” situation-based entry.
- Simple visual hierarchy.
- Hindi support and Devanagari typography.
- Voice/read-aloud direction.
- Four-step progress indicator.
- Review and edit step.
- Safety Tips and suspect lookup concepts.
- Modern card-based visual language.
- Lightweight demo startup.

## 22. Suggested implementation order

### First priority

1. Reset form state between journeys.
2. Fix mobile overflow.
3. Add category-specific report forms.
4. Add immediate-action panels.
5. Add the evidence checklist.
6. Make demo mode obvious.
7. Make cards and upload keyboard-accessible.

### Second priority

8. Add help for technical terms.
9. Complete Hindi localization.
10. Add privacy choices and safer sensitive-content handling.
11. Add “what happens next?” timeline.
12. Improve tracking with sample states.

### Third priority

13. Add guided “Help me decide” mode.
14. Add age/digital-literacy assistance modes.
15. Add report-suspect flow.
16. Add FAQ/contact/policy content.
17. Add presenter-friendly demo reset and sample journeys.

## Final target

The improved demo should make this journey obvious for every user:

```text
Something happened to me
        ↓
I understand what it is
        ↓
I know what to do immediately
        ↓
I know what evidence to keep
        ↓
I can report without confusion
        ↓
I understand privacy and next steps
        ↓
I can find my complaint later
```

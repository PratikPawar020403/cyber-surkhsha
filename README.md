# CyberSuraksha (NCRP 2.0)

> A modern, citizen-first redesign of India's National Cyber Crime Reporting Portal (`cybercrime.gov.in`).  
> Built for the **"Redesign Indian Sites"** competition.

---

## 🚀 Key Features

- **🎙️ AI Voice & Text Complaint Intake**: Speak or type in plain language. Built-in NLP automatically detects crime categories, extracts entities (amounts, phone numbers, UPI IDs, links), and auto-populates the reporting wizard.
- **⚡ 90-Second 4-Step Reporting**: Replaces 35+ rigid legal form fields and multi-tab registration barriers with progressive disclosure, reducing filing time from ~30 minutes to under 90 seconds.
- **🛡️ 1930 "Golden Hour" Helpline Escalation**: Persistent 1-tap dialer access across header, hero, and mobile views to fast-track bank lien freezes before mule transfers occur.
- **📜 Automated RBI 3-Day Zero-Liability Dispute Letter**: Instantly generates a print-ready statutory legal notice citing RBI Circular *DBR.No.Leg.BC.78/09.07.005/2017-18* for mandatory bank refund processing.
- **🔍 Suspect Telemetry Scanner**: Instant cross-referencing against flagged mule numbers, UPI IDs, and malicious APK links with normalized search.
- **📊 4-Stage Live Tracking & Officials Queue**: Visual case lifecycle timeline (`Received` → `Assigned` → `Investigation` → `Resolved`) synced with a simulated Cyber Cell officer triage dashboard.
- **♿ Inclusive Accessibility (GIGW 3.0 / WCAG 2.2 AA)**: Pure black-and-white High Contrast mode, font-size scaler (`A-` / `A+`), Web Speech voice reader FAB, and matra-preserved Devanagari Hindi typography.

---

## 🛠️ Architecture

- **Zero-Dependency Vanilla Web Stack**: Single-file `<180 KB` HTML5, CSS3, and JavaScript architecture with instant 3G load times.
- **Client-Side Storage Engine**: Resilient `localStorage` wrapper with in-memory fallbacks for offline demo reliability.
- **Native Web APIs**: Utilizes Web Speech Recognition, Web Speech Synthesis, and HTML5 Drag-and-Drop file handling.

---

## 💻 Run Locally

No build steps or package managers required. Simply open `index.html` directly in any modern browser, or run a local server:

```bash
# Using Python
python -m http.server 8000

# Or using Node.js
npx serve .
```

Visit: `http://localhost:8000`

---

*Disclaimer: This is a UI/UX concept prototype for competition evaluation. Complaint routing and telemetry ingestion are simulated for demonstration purposes.*

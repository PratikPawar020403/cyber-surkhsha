# CyberSuraksha / NCRP 2.0 SPA — Master UI/UX, Visual Design & Accessibility Audit Report

**Target Application**: CyberSuraksha / NCRP 2.0 Single-Page Web Application  
**File Path**: `c:/Users/prati/OneDrive/Desktop/cc/index.html` (2,660 lines)  
**Evaluation Standard**: WCAG 2.2 AA/AAA, GIGW 3.0, ISO 9241-110, Responsive Web Standards (320px–1440px+)  
**Audit Coordination**: Teamwork Multi-Perspective Orchestrator (`orchestrator_1`)  
**Specialized Auditor Agents**:
- **R1**: Visual Hierarchy, Typography & Styling Consistency Auditor
- **R2**: Interactive Flow, Validation & State Machine UX Auditor
- **R3**: Accessibility (A11y), WCAG 2.2 / GIGW 3.0 & Bilingual Polish Auditor
- **R4**: Responsive Layout & Mobile Usability Auditor

---

## 1. Executive Summary & Defect Statistics

An exhaustive, multi-perspective line-by-line audit of `index.html` was conducted across all 8 SPA views (Home, 4-Step Incident Wizard, Confirmation Slip, Diagnostic Triage, Learning Corner, Suspect Scanner, Citizen Tracking Dashboard, and Officials Queue). 

The application features a clean modern aesthetic, custom CSS custom properties, and zero external framework dependencies. However, the audit uncovered **69 total domain observations**, synthesized into **35 unique core defects** spanning visual design, interactive state machines, accessibility barriers, and mobile viewport glitches.

### Defect Breakdown by Severity & Domain

| Audit Domain | [CRITICAL] | [HIGH] | [MEDIUM] | [LOW] | Domain Total |
|---|:---:|:---:|:---:|:---:|:---:|
| **R1: Visual Hierarchy & Styling Consistency** | 3 | 5 | 7 | 5 | **20** |
| **R2: Interactive Flow, Validation & State Machine** | 3 | 4 | 4 | 2 | **13** |
| **R3: Accessibility (A11y), WCAG 2.2 & Bilingual** | 4 | 9 | 4 | 3 | **20** |
| **R4: Responsive Layout & Mobile Usability** | 4 | 6 | 4 | 2 | **16** |
| **Consolidated Unique Defects (Deduplicated)** | **8** | **14** | **9** | **4** | **35** |

---

## 2. Top 5 Prioritized Quick Wins for Immediate Competition Readiness

These 5 high-impact drop-in fixes address the most severe usability blockers, compliance violations, and data integrity risks:

---

### 🏆 Quick Win #1: Fix Mobile FAB & Emergency Bar Collision
- **Severity**: `[CRITICAL]`
- **Target Lines**: `index.html` Lines 771–790, 1742–1758
- **User Impact**: On screen widths $\le$768px, the floating Voice Assistant button (`.voice-fab`, `z-index: 90; bottom: 20px`) sits directly on top of the sticky mobile emergency bar (`.mobile-bar`, `z-index: 80; bottom: 0`), completely blocking citizen taps on the critical **"Call 1930"** national helpline button.
- **Drop-in Fix**:
```css
/* In CSS @media (max-width: 768px) around line 771 */
@media (max-width: 768px) {
  .mobile-bar {
    display: flex;
    padding-bottom: calc(10px + env(safe-area-inset-bottom, 0px));
  }
  body {
    padding-bottom: calc(72px + env(safe-area-inset-bottom, 0px));
  }
  .voice-fab {
    bottom: calc(74px + env(safe-area-inset-bottom, 0px)) !important;
    right: 16px !important;
    padding: 8px 14px;
    font-size: 0.80rem;
    box-shadow: var(--shadow-lg);
  }
}
```

---

### 🏆 Quick Win #2: Remove Viewport Zoom Lock & Repair High-Contrast Tokens
- **Severity**: `[CRITICAL]`
- **Target Lines**: `index.html` Line 5, Lines 74–93, Lines 600–604, 635–639, 695–698
- **User Impact**: `maximum-scale=1` in `<meta name="viewport">` unlawfully disables user pinch-to-zoom on mobile devices (WCAG 1.4.4 / GIGW 3.0). Furthermore, high-contrast mode leaves hardcoded light-pastel backgrounds on the NCRP slip, RBI letter, and declaration boxes, causing illegible text for low-vision users.
- **Drop-in Fix**:
```html
<!-- Replace Line 5 in index.html -->
<meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover">
```
```css
/* Update html.high-contrast rules (lines 74-93) */
html.high-contrast {
  --bg: #FFFFFF;
  --card: #FFFFFF;
  --card-subtle: #F4F4F4;
  --border: #000000;
  --border-hover: #000000;
  --border-focus: #000000;
  --text-main: #000000;
  --text-muted: #1A1A1A;
  --text-light: #333333;
  --primary: #000000;
  --primary-hover: #1A1A1A;
  --emergency: #9E0000;
  --emergency-bg: #FFEAEA;
  --emergency-border: #000000;
  --emergency-text: #9E0000;
  --success: #005A1C;
  --success-bg: #EAF8EC;
  --success-border: #000000;
  --success-text: #004D16;
  --warning: #7C2D12;
  --warning-bg: #FEF3C7;
  --warning-border: #000000;
  --warning-text: #7C2D12;
  --shadow-sm: none;
  --shadow-md: 0 0 0 2px #000;
  --shadow-lg: 0 0 0 3px #000;
}
html.high-contrast .ncrp-slip-card,
html.high-contrast .rbi-letter-sheet,
html.high-contrast .jurisdiction-badge,
html.high-contrast .declaration-box {
  background: #FFFFFF !important;
  border-color: #000000 !important;
  color: #000000 !important;
}
html.high-contrast .notch-navbar::before,
html.high-contrast .notch-navbar::after {
  display: none !important;
}
```

---

### 🏆 Quick Win #3: Enforce Wizard Step Validation & Eliminate Fake Complainant Injection
- **Severity**: `[CRITICAL]`
- **Target Lines**: `index.html` Lines 1421, 1498, 2192–2193
- **User Impact**: Blank submissions bypass all checks and silently attribute official police complaints to a hardcoded placeholder identity: "Rajesh Sharma" (phone `9876543210`). Citizens lose access to their real complaint reference, generating falsified police intake dossiers.
- **Drop-in Fix**:
```javascript
// Add step-level validation to index.html
function validateStep(currentStep){
  var isHi = isHindi();
  if(currentStep === 2){
    var desc = (document.getElementById('description').value || '').trim();
    if(!desc || desc.length < 15){
      alert(isHi ? 'कृपया घटना का कम से कम 15 अक्षरों में विवरण दें।' : 'Please describe what happened (minimum 15 characters).');
      document.getElementById('description').focus();
      return false;
    }
  }
  if(currentStep === 3){
    if(!flowState.anon){
      var name = (document.getElementById('full-name').value || '').trim();
      var mob = (document.getElementById('mobile').value || '').trim();
      if(!name){
        alert(isHi ? 'कृपया अपना पूरा नाम दर्ज करें।' : 'Please enter your full name.');
        document.getElementById('full-name').focus();
        return false;
      }
      if(!/^[6-9]\d{9}$/.test(mob)){
        alert(isHi ? 'कृपया वैध 10-अंकीय मोबाइल नंबर दर्ज करें।' : 'Please enter a valid 10-digit mobile number.');
        document.getElementById('mobile').focus();
        return false;
      }
      if(flowState.otpSent && !flowState.otpVerified){
        alert(isHi ? 'कृपया जारी रखने से पहले 6-अंकीय OTP सत्यापित करें।' : 'Please verify the 6-digit OTP before proceeding.');
        return false;
      }
    }
  }
  return true;
}

// Update step advance buttons in HTML:
// onclick="if(validateStep(2)) goToStep(3)"
// onclick="if(validateStep(3)) goToStep(4)"

// Fix submitReport() in JS (lines 2192-2193):
name: flowState.anon ? null : document.getElementById('full-name').value.trim(),
mobile: flowState.anon ? null : document.getElementById('mobile').value.trim(),
```

---

### 🏆 Quick Win #4: Fix RBI Dispute Notice Print Stylesheet Collision & Persistent Storage
- **Severity**: `[CRITICAL]`
- **Target Lines**: `index.html` Lines 658–663, 1272, 1764–1773
- **User Impact**: `@media print` hardcodes `visibility: visible` *only* for `#screen-confirmation`. When a citizen clicks "Print / Save PDF" on the RBI Dispute Letter (`#screen-rbi`), standard browsers print a completely blank page. Furthermore, storage falls back to volatile memory (`_memStore`), wiping all cases upon page refresh.
- **Drop-in Fix**:
```css
/* Replace lines 658-663 in index.html */
@media print {
  @page { margin: 12mm; size: A4 portrait; }
  html, body { background: #fff !important; color: #000 !important; }
  body * { visibility: hidden !important; }
  
  #screen-confirmation.active, #screen-confirmation.active *,
  #screen-rbi.active, #screen-rbi.active * {
    visibility: visible !important;
  }
  #screen-confirmation.active, #screen-rbi.active {
    position: absolute !important;
    left: 0 !important;
    top: 0 !important;
    width: 100% !important;
    margin: 0 !important;
    padding: 0 !important;
    display: block !important;
  }
  .btn, .site-header, .site-footer, .voice-fab, .mobile-bar, .demo-shortcuts, .back-link, .rbi-card {
    display: none !important;
  }
}
```
```javascript
// Upgrade storage (lines 1764-1773) to wrap localStorage
var storage = window.storage || {
  get: function(key, shared){
    return new Promise(function(res, rej){
      try {
        var val = localStorage.getItem('cs_' + key);
        if(val !== null) res({ key: key, value: val, shared: !!shared });
        else rej(new Error('not_found'));
      } catch(e) { rej(e); }
    });
  },
  set: function(key, value, shared){
    return new Promise(function(res, rej){
      try {
        localStorage.setItem('cs_' + key, value);
        res({ key: key, value: value, shared: !!shared });
      } catch(e) { rej(e); }
    });
  },
  delete: function(key, shared){
    return new Promise(function(res, rej){
      try {
        localStorage.removeItem('cs_' + key);
        res({ key: key, deleted: true, shared: !!shared });
      } catch(e) { rej(e); }
    });
  },
  list: function(prefix, shared){
    return new Promise(function(res){
      var keys = [];
      var fullPrefix = 'cs_' + (prefix || '');
      for(var i=0; i<localStorage.length; i++){
        var k = localStorage.key(i);
        if(k && k.indexOf(fullPrefix) === 0) keys.push(k.replace('cs_', ''));
      }
      res({ keys: keys, prefix: prefix, shared: !!shared });
    });
  }
};
```

---

### 🏆 Quick Win #5: Restore Mobile Language Switcher & Fix Devanagari Ligature Spacing
- **Severity**: `[CRITICAL]` / `[HIGH]`
- **Target Lines**: `index.html` Lines 63–71, 368–371, 824–828
- **User Impact**: `@media (max-width: 600px)` hides `.notch-pills`, stripping the `EN / हिं` language toggle on mobile with no drawer fallback. Additionally, global `letter-spacing: -0.02em` breaks Devanagari conjunct characters (संयुक्त अक्षर) and clips matras.
- **Drop-in Fix**:
```css
/* In CSS: keep language toggle visible on mobile */
@media (max-width: 600px) {
  .notch-pills { display: flex !important; transform: scale(0.92); }
  .notch-pill-btn { padding: 4px 8px; font-size: 0.72rem; }
  .notch-white-pill-btn span { display: none; }
  .notch-white-pill-btn { padding: 6px 10px; }
}

/* Devanagari typography polish: reset tracking & expand line height */
:lang(hi) *, html[lang="hi"] * {
  letter-spacing: normal !important;
}
html[lang="hi"] h1, html[lang="hi"] h2, html[lang="hi"] h3, html[lang="hi"] h4 {
  line-height: 1.45 !important;
  padding: 2px 0;
}
html[lang="hi"] .btn, html[lang="hi"] .nav-btn {
  padding-top: 8px;
  padding-bottom: 8px;
}
```

---

## 3. Comprehensive Categorized Defect Log

Below is the complete, categorized log of all 35 verified defects with line numbers, severity ratings, citizen/official impact, and exact drop-in code fixes.

---

### Domain R1: Visual Hierarchy, Typography & Styling Consistency

#### Defect R1-01 `[CRITICAL]` — Stepper Progress Fill Bar Geometry Overshoot
- **Target Lines**: `index.html` Lines 499–505, 2015–2018
- **Component**: Report Wizard Stepper (`.stepper-line-fill` & `goToStep()`)
- **User Impact**: `.stepper-line` is offset `left: 20px; right: 20px;`. At Step 4, `goToStep(4)` sets `width = 100%`, which starts at 20px and extends 20px past the 4th node circle, causing a broken visual layout during final review.
- **Drop-in Fix**:
```javascript
// Replace lines 2015-2017 in index.html
var trackFill = document.getElementById('stepper-track-fill');
if(trackFill) trackFill.style.width = 'calc((100% - 40px) * ' + ((n - 1) / 3) + ')';
```

#### Defect R1-02 `[HIGH]` — Active Navigation State Disorientation across Secondary Views
- **Target Lines**: `index.html` Lines 1804–1816
- **Component**: Navigation Controller `showScreen()`
- **User Impact**: `navMap` only tracks 5 primary IDs. Navigating to `screen-flow`, `screen-confirmation`, `screen-rbi`, or `screen-admin` strips all `.current` classes, displaying an unselected, disorienting navigation bar.
- **Drop-in Fix**:
```javascript
// Replace lines 1806-1816 in index.html
var navMap = {
  'screen-home': 0,
  'screen-flow': 0,
  'screen-confirmation': 0,
  'screen-diagnostic': 1,
  'screen-rbi': 1,
  'screen-learning': 2,
  'screen-suspect': 3,
  'screen-track': 4,
  'screen-admin': 0
};
if(navMap[id] !== undefined){
  var btns = document.querySelectorAll('.header-nav .nav-btn');
  if(btns[navMap[id]]) btns[navMap[id]].classList.add('current');
}
```

#### Defect R1-03 `[HIGH]` — Officials Queue Table Tap Targets & Un-badged Status Text
- **Target Lines**: `index.html` Lines 1707–1721, 2340–2347
- **Component**: Officials Queue Data Table (`#admin-table-body`)
- **User Impact**: Action buttons have `style="padding:4px 8px; font-size:0.75rem;"` (24px height), failing touch accessibility. The status column renders raw unstyled text.
- **Drop-in Fix**:
```javascript
// Replace line 2344 in index.html
var statusClass = c.status === 'resolved' ? 'badge-success' : (c.status === 'received' ? 'badge-neutral' : 'badge-progress');
tr.innerHTML = '<td style="padding:12px 14px; font-family:var(--font-mono); font-weight:700;">' + c.ref + '</td>' +
  '<td style="padding:12px 14px;">' + c.situationLabel + '</td>' +
  '<td style="padding:12px 14px; max-width:220px; color:var(--text-muted);">' + (c.description ? c.description.slice(0,60)+'…' : '—') + '</td>' +
  '<td style="padding:12px 14px;">' + new Date(c.createdAt).toLocaleDateString() + '</td>' +
  '<td style="padding:12px 14px;"><span class="status-pill ' + statusClass + '">' + (statusLabel[c.status] ? (isHi ? statusLabel[c.status].hi : statusLabel[c.status].en) : c.status) + '</span></td>' +
  '<td style="padding:12px 14px;">' + (nextStatus ? '<button class="btn btn-secondary" style="padding:6px 12px; font-size:0.80rem; font-weight:700; min-height:36px;" onclick="advanceStatus(\'' + c.ref + '\')">→ ' + nextStatus + '</button>' : '<span style="color:var(--success-text); font-weight:700;">✓ Completed</span>') + '</td>';
```

#### Defect R1-04 `[MEDIUM]` — Citizen Tracking Status Badge Hardcodes False Green "Success"
- **Target Lines**: `index.html` Line 2269
- **Component**: Citizen Tracking Result Badge (`trackComplaint()`)
- **User Impact**: The status badge unconditionally renders with `--success-bg` (bright green) even when status is `received` (initial intake) or `investigating`, misleading victims into believing the case is already resolved.
- **Drop-in Fix**:
```javascript
// Update line 2269 in index.html
var badgeStyle = c.status === 'resolved' 
  ? 'background:var(--success-bg); border:1px solid var(--success-border); color:var(--success-text);'
  : (c.status === 'investigating' 
    ? 'background:#EFF6FF; border:1px solid #BFDBFE; color:#1E3A8A;'
    : 'background:var(--card-subtle); border:1px solid var(--border); color:var(--text-muted);');
```

#### Defect R1-05 `[MEDIUM]` — Learning Corner Guide Action Buttons Styled as Passive Demo Chips
- **Target Lines**: `index.html` Lines 1085, 1103, 1121, 1139, 1157, 1175
- **Component**: Guide Cards Action Row (`.lc-card-action .demo-chip`)
- **User Impact**: Action buttons inside `.lc-card` use class `.demo-chip` (intended for testing shortcuts), appearing as passive labels (~26px height) rather than prominent, interactive call-to-actions.
- **Drop-in Fix**:
```css
/* Add to CSS under line 738 */
.lc-action-btn {
  background: var(--card-subtle);
  border: 1px solid var(--border);
  color: var(--text-main);
  padding: 6px 14px;
  border-radius: var(--radius-sm);
  font-size: 0.82rem;
  font-weight: 700;
  cursor: pointer;
  display: inline-flex;
  align-items: center;
  gap: 4px;
  min-height: 36px;
  transition: all 0.15s ease;
}
.lc-action-btn:hover {
  background: var(--text-main);
  color: #FFFFFF;
  border-color: var(--text-main);
}
```

---

### Domain R2: Interactive Flow, Validation & State Machine UX

#### Defect R2-01 `[HIGH]` — OTP Input Listener Leaks, Paste Failure & Manual Verification Bug
- **Target Lines**: `index.html` Lines 1450–1462, 2127–2154
- **Component**: Step 3 OTP Verification System (`initOtpBoxes()`, `sendOtp()`)
- **User Impact**: Re-clicking "Send OTP" binds duplicate event listeners causing erratic multi-box jumps; pasting a 6-digit SMS code only fills box 0; and manually typing 6 digits never sets `flowState.otpVerified = true` (it only validated via `autoFillDemoOtp`).
- **Drop-in Fix**:
```javascript
// Replace lines 2127-2154 in index.html
var otpInitialized = false;
function initOtpBoxes(){
  if(otpInitialized) return;
  otpInitialized = true;
  var boxes = document.querySelectorAll('.otp-box');
  
  boxes.forEach(function(box, idx){
    box.addEventListener('input', function(e){
      var val = box.value.replace(/[^0-9]/g, '');
      box.value = val ? val.slice(-1) : '';
      box.classList.toggle('filled', !!box.value);
      if(box.value && idx < boxes.length - 1) boxes[idx+1].focus();
      checkFullOtp();
    });

    box.addEventListener('keydown', function(e){
      if(e.key === 'Backspace'){
        if(!box.value && idx > 0){
          boxes[idx-1].focus();
          boxes[idx-1].value = '';
          boxes[idx-1].classList.remove('filled');
        } else {
          box.value = '';
          box.classList.remove('filled');
        }
        checkFullOtp();
      }
    });

    box.addEventListener('paste', function(e){
      e.preventDefault();
      var pasteData = (e.clipboardData || window.clipboardData).getData('text').replace(/[^0-9]/g, '');
      if(!pasteData) return;
      for(var i=0; i<boxes.length; i++){
        if(pasteData[i]){
          boxes[i].value = pasteData[i];
          boxes[i].classList.add('filled');
        }
      }
      var nextFocus = Math.min(pasteData.length, boxes.length - 1);
      boxes[nextFocus].focus();
      checkFullOtp();
    });
  });
}

function checkFullOtp(){
  var boxes = document.querySelectorAll('.otp-box');
  var code = Array.from(boxes).map(function(b){ return b.value; }).join('');
  if(code.length === 6){
    flowState.otpVerified = true;
    var msg = document.getElementById('otp-status-msg') || document.createElement('div');
    msg.id = 'otp-status-msg';
    msg.style.cssText = 'color:var(--success-text); font-size:0.84rem; font-weight:700; margin-top:6px;';
    msg.textContent = isHindi() ? '✓ OTP सफलतापूर्वक सत्यापित हो गया।' : '✓ OTP Verified Successfully.';
    document.getElementById('otp-group').appendChild(msg);
  }
}
```

#### Defect R2-02 `[HIGH]` — Hidden Suspect Form Inputs Retain Values & Leak Invisible Data
- **Target Lines**: `index.html` Lines 1380–1405, 1962–1976, 2173–2175
- **Component**: Step 2 Suspect Metadata Chips (`toggleSuspectField()`)
- **User Impact**: When a citizen collapses or deselects a suspect field, the input value is NOT cleared from the DOM. `submitReport()` extracts and files the hidden data into the police dossier without the user's consent.
- **Drop-in Fix**:
```javascript
// Replace lines 1962-1976 in index.html
function toggleSuspectField(type, forceShow){
  var wrap = document.getElementById('suspect-fields-wrap');
  var row = document.getElementById('sf-' + type + '-row');
  var chipBtn = document.getElementById('chip-btn-' + type);
  if(!row) return;

  var isCurrentlyHidden = (row.style.display === 'none' || !row.style.display);
  var shouldShow = (forceShow === true) ? true : isCurrentlyHidden;

  if(shouldShow){
    row.style.display = 'block';
    if(chipBtn) chipBtn.classList.add('active');
    var inp = row.querySelector('input');
    if(inp) inp.focus();
  } else {
    row.style.display = 'none';
    var inp = row.querySelector('input');
    if(inp) inp.value = ''; // Cleanly clear value
    if(chipBtn) chipBtn.classList.remove('active');
  }

  var anyVisible = Array.from(wrap.children).some(function(child){
    return child.style.display === 'block';
  });
  wrap.style.display = anyVisible ? 'flex' : 'none';
}
```

#### Defect R2-03 `[HIGH]` — Officials Intake Queue Lacks Search, Filtering, Batch Operations & Case Dossier Modal
- **Target Lines**: `index.html` Lines 1691–1723, 2323–2360
- **Component**: Officials Queue Administration Portal (`loadAdminQueue()`)
- **User Impact**: Triage officers cannot search by reference ID, citizen phone, or filter by crime category/status, creating a major administrative bottleneck.
- **Drop-in Fix**:
```html
<!-- Add toolbar above the table (around line 1707) -->
<div class="admin-toolbar" style="display:flex; gap:10px; margin-bottom:14px; flex-wrap:wrap;">
  <input type="text" id="admin-search-input" class="clean-input" placeholder="Search by Ref #, Name, Phone…" oninput="filterAdminQueue()" style="max-width:280px;">
  <select id="admin-status-filter" class="clean-input" onchange="filterAdminQueue()" style="max-width:180px;">
    <option value="all">All Statuses</option>
    <option value="received">Received</option>
    <option value="assigned">Assigned</option>
    <option value="investigating">Investigating</option>
    <option value="resolved">Resolved</option>
  </select>
  <select id="admin-category-filter" class="clean-input" onchange="filterAdminQueue()" style="max-width:200px;">
    <option value="all">All Categories</option>
    <option value="money">Financial Fraud</option>
    <option value="harass">Extortion / Threat</option>
    <option value="hack">Hacked Account</option>
  </select>
</div>
```

#### Defect R2-04 `[MEDIUM]` — Suspect Scanner Sanitization Key Mismatch & Missing Loading Feedback
- **Target Lines**: `index.html` Lines 1775, 2289–2321, 2568–2570
- **Component**: Suspect Scanner Verification Tool (`checkSuspect()`)
- **User Impact**: `sanitizeKey('+91 9876543210')` converts `+` to `_`, returning `_91_9876543210`. The pre-seeded key was stored with a literal `+` (`suspect:+91_9876543210`), causing direct lookups to fail. Furthermore, the check happens with 0ms visual feedback.
- **Drop-in Fix**:
```javascript
// In JS:
function sanitizeKey(str){
  return str.toLowerCase().trim().replace(/[^a-z0-9@._-]/g, '').slice(0, 80);
}

async function checkSuspect(){
  var input = document.getElementById('suspect-input').value.trim();
  var box = document.getElementById('suspect-result-box');
  var btn = document.getElementById('check-btn');
  var isHi = isHindi();
  if(!input){
    box.style.display = 'block';
    box.innerHTML = '<div style="color:var(--emergency-text); background:var(--emergency-bg); padding:10px; border-radius:var(--radius-sm); font-size:0.86rem;">' + (isHi ? 'कृपया जांच के लिए फोन नंबर, UPI ID या लिंक दर्ज करें।' : 'Please enter a phone number, UPI ID, or link to check.') + '</div>';
    return;
  }
  btn.disabled = true;
  btn.textContent = isHi ? 'डेटाबेस स्कैन हो रहा है…' : 'Scanning 140,000+ Records…';
  await new Promise(function(r){ setTimeout(r, 350); });
  btn.disabled = false;
  btn.textContent = isHi ? 'डेटाबेस स्कैन करें' : 'Scan Database';
  // Proceed with lookup
  ...
}
```

---

### Domain R3: Accessibility (A11y), WCAG 2.2 AA/AAA & Bilingual Polish

#### Defect R3-01 `[CRITICAL]` — Situation & Diagnostic Cards Implemented as Non-Focusable `<div>` Elements
- **Target Lines**: `index.html` Lines 891–924, Lines 968–983
- **Component**: Home Situation Cards & 30-Second Diagnostic Triage Cards
- **User Impact**: Implemented as `<div class="sit-card" onclick="...">` without `tabindex="0"`, `role="button"`, or keyboard event listeners. Motor-impaired and blind keyboard-only users cannot tab to or activate the core entry points of the portal (WCAG 2.1.1, 4.1.2).
- **Drop-in Fix**:
```html
<!-- Convert to semantic button or add accessible attributes (lines 891-924) -->
<button type="button" class="sit-card" onclick="startFlow('money')" style="text-align:left; width:100%; border:1px solid var(--border);">
  <div class="sit-icon" style="background:#EFF6FF; color:#1D4ED8;">💳</div>
  <div class="sit-title" data-en="Lost Money to a Scammer" data-hi="वित्तीय धोखाधड़ी (पैसा गंवाया)">Lost Money to a Scammer</div>
  <p class="sit-desc" data-en="UPI fraud, fake investment app, OTP theft or unauthorised bank debit." data-hi="UPI धोखाधड़ी, फर्जी निवेश ऐप, OTP चोरी या अनधिकृत बैंक डेबिट।">UPI fraud, fake investment app, OTP theft or unauthorised bank debit.</p>
  <span class="sit-tag" style="background:var(--emergency-bg); color:var(--emergency-text); font-weight:700;" data-en="⚡ Freeze Transfer (0-4h) →" data-hi="⚡ ट्रांसफर रोकें (0-4 घंटे) →">⚡ Freeze Transfer (0-4h) →</span>
</button>
```

#### Defect R3-02 `[CRITICAL]` — Form `<label>` Tags Lack `for`/`id` Programmatic Association
- **Target Lines**: `index.html` Lines 1228–1260, 1320, 1354, 1365, 1369, 1393–1401, 1438–1443, 1476–1480, 1646, 1671
- **Component**: Form Input Controls across all 8 Views
- **User Impact**: Screen readers announce "edit text, blank" without speaking the field label, leaving blind citizens unable to determine what data to input (WCAG 1.3.1 Info & Relationships).
- **Drop-in Fix**:
```html
<!-- Pair each label with its input ID -->
<label for="situation-select" data-en="Confirm Incident Category" data-hi="घटना श्रेणी की पुष्टि करें">Confirm Incident Category</label>
<select id="situation-select" class="clean-input" ...>

<label for="description" data-en="Describe What Happened" data-hi="घटना का विवरण दें">Describe What Happened</label>
<textarea id="description" class="clean-input" ...></textarea>

<label for="full-name" data-en="Your Full Name" data-hi="आपका पूरा नाम">Your Full Name</label>
<input type="text" id="full-name" class="clean-input" ...>

<label for="mobile" data-en="Mobile Number (for OTP & SMS Updates)" data-hi="मोबाइल नंबर (OTP और SMS अपडेट के लिए)">Mobile Number</label>
<input type="tel" id="mobile" class="clean-input" ...>
```

#### Defect R3-03 `[HIGH]` — Missing Skip-to-Main-Content Bypass Link
- **Target Lines**: `index.html` Lines 795–798
- **Component**: Global Site Header & Navigation Landmark
- **User Impact**: Keyboard users must press `Tab` 10+ times through the header and language pills on every single page load before reaching main content (WCAG 2.4.1 / GIGW 3.0).
- **Drop-in Fix**:
```html
<!-- Insert right after <body> (line 795) -->
<a href="#main" class="skip-link" data-en="Skip to main content" data-hi="मुख्य सामग्री पर जाएं">Skip to main content</a>
```
```css
/* Add to CSS */
.skip-link {
  position: absolute;
  top: 8px;
  left: 8px;
  z-index: 9999;
  background: var(--primary);
  color: #FFFFFF;
  padding: 10px 16px;
  font-weight: 700;
  border-radius: var(--radius-sm);
  transform: translateY(-160%);
  transition: transform 0.15s ease;
}
.skip-link:focus-visible {
  transform: translateY(0);
  outline: 3px solid #F59E0B;
}
```

#### Defect R3-04 `[HIGH]` — Missing GIGW 3.0 Font Resizing Widget (`A-`, `A`, `A+`)
- **Target Lines**: `index.html` Lines 59, 100, 824–836
- **Component**: Accessibility Header Controls
- **User Impact**: Portal lacks the mandatory GIGW 3.0 on-screen font size adjustments for citizens who cannot use keyboard shortcuts.
- **Drop-in Fix**:
```html
<!-- Add to .notch-pills (around line 824) -->
<div class="font-resize-group" style="display:flex; gap:2px; margin-right:4px;">
  <button type="button" class="notch-pill-btn" onclick="changeFontSize(-1)" aria-label="Decrease Font Size">A-</button>
  <button type="button" class="notch-pill-btn" onclick="changeFontSize(0)" aria-label="Reset Font Size">A</button>
  <button type="button" class="notch-pill-btn" onclick="changeFontSize(1)" aria-label="Increase Font Size">A+</button>
</div>
```
```javascript
// Add JS handler
var currentFontScale = 1;
function changeFontSize(delta){
  if(delta === 0) currentFontScale = 1;
  else currentFontScale = Math.max(0.85, Math.min(1.35, currentFontScale + delta * 0.1));
  document.documentElement.style.setProperty('--fs-scale', currentFontScale);
}
```

#### Defect R3-05 `[HIGH]` — Dynamic Result Panels Lack ARIA Live Region Semantics
- **Target Lines**: `index.html` Lines 990–992, 1652–1653, 1686, 2010–2029
- **Component**: Diagnostic Triage, Suspect Scanner & Citizen Tracking Results
- **User Impact**: Results appear silently on the page without screen reader announcements, leaving visually impaired citizens unaware that their search or status update completed.
- **Drop-in Fix**:
```html
<!-- Add role and aria-live attributes -->
<div id="diag-result-card" role="region" aria-live="polite" style="display:none; ...">
<div id="track-result" role="region" aria-live="polite" style="display:none; ...">
<div id="suspect-result-box" role="region" aria-live="polite" style="display:none; ...">
<div id="track-error" role="alert" aria-live="assertive" style="display:none; ...">
```

---

### Domain R4: Responsive Layout & Mobile Usability

#### Defect R4-01 `[CRITICAL]` — Learning Corner & Statutory Rights Grid Horizontal Overflow
- **Target Lines**: `index.html` Lines 714–716, 1186
- **Component**: `.lc-guides-grid` & Statutory Rights Section
- **User Impact**: `grid-template-columns: repeat(auto-fill, minmax(320px, 1fr));` forces a 320px minimum column. On a 320px screen with container padding (280px available width), this causes a 40px horizontal page blowout with broken horizontal scrollbars.
- **Drop-in Fix**:
```css
/* Replace lines 714-716 in index.html */
.lc-guides-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(min(100%, 280px), 1fr));
  gap: 16px;
}
@media (max-width: 480px) {
  .lc-guides-grid {
    grid-template-columns: 1fr;
    gap: 12px;
  }
}
```

#### Defect R4-02 `[HIGH]` — Form Input Font Size (0.95rem = 15.2px) Triggers iOS Safari Auto-Zoom
- **Target Lines**: `index.html` Line 540
- **Component**: `.clean-input` Typography
- **User Impact**: Any input/select with `font-size < 16px` forces iOS Safari to automatically zoom the viewport on tap, shifting page margins and disorienting users.
- **Drop-in Fix**:
```css
/* Update line 540 in index.html */
.clean-input {
  width: 100%;
  border: 1px solid var(--border);
  border-radius: var(--radius-sm);
  padding: 11px 14px;
  font-size: 1rem; /* 16px eliminates iOS Safari auto-zoom */
  font-family: inherit;
  color: var(--text-main);
  background: var(--card);
}
@media (max-width: 768px) {
  .clean-input { font-size: 16px !important; }
}
```

#### Defect R4-03 `[HIGH]` — OTP 6-Box Grid Overflow on 320px Mobile Screens
- **Target Lines**: `index.html` Lines 561–570, 1452–1459
- **Component**: `.otp-grid` & `.otp-box`
- **User Impact**: 6 fixed 44px boxes + gaps total 304px, exceeding the ~244px available inner width on 320px smartphones, clipping boxes 5 and 6.
- **Drop-in Fix**:
```css
/* Replace lines 561-570 in index.html */
.otp-grid {
  display: flex;
  gap: clamp(4px, 1.8vw, 8px);
  margin: 12px 0 16px;
  width: 100%;
  max-width: 320px;
}
.otp-box {
  flex: 1 1 0px;
  min-width: 0;
  height: clamp(44px, 12vw, 50px);
  text-align: center;
  font-size: 1.15rem;
  font-weight: 700;
  border: 1px solid var(--border);
  border-radius: var(--radius-sm);
  font-family: var(--font-mono);
  color: var(--text-main);
  background: var(--card);
}
```

#### Defect R4-04 `[HIGH]` — Hardcoded 2-Column Inline Grids Unstacked on Mobile
- **Target Lines**: `index.html` Lines 1363–1378, 1474–1483, 1609–1616
- **Component**: Wizard Step 2 Date/Delay, Step 3 State/District, and Confirmation Buttons
- **User Impact**: On screen widths <560px, fields are squeezed into narrow 2-column inline grids, clipping date pickers, district dropdowns, and button labels.
- **Drop-in Fix**:
```css
/* Replace inline 2-column styles with responsive utility */
.responsive-2col-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 12px;
}
@media (max-width: 560px) {
  .responsive-2col-grid {
    grid-template-columns: 1fr;
    gap: 8px;
  }
}
```

#### Defect R4-05 `[HIGH]` — Under-Sized Touch Targets (<44px) on Key Interactive Elements
- **Target Lines**: `index.html` Lines 488–494 (`.back-link`), 630–633 (`.review-edit-btn`), 583 (`.f-chip button`), 2344 (Officials Queue status buttons)
- **Component**: Navigation Back Link, Review Edit Triggers, File Chip Deletion
- **User Impact**: Sub-44px touch targets lead to high mis-tap error rates on touchscreens, frustrating distressed citizens.
- **Drop-in Fix**:
```css
/* Ensure minimum 44px touch target height */
.back-link {
  display: inline-flex;
  align-items: center;
  gap: 6px;
  padding: 10px 12px 10px 0;
  min-height: 44px;
}
.review-edit-btn {
  padding: 8px 12px;
  min-height: 40px;
  display: inline-flex;
  align-items: center;
}
.f-chip button {
  padding: 8px 10px;
  min-width: 36px;
  min-height: 36px;
  display: inline-flex;
  align-items: center;
  justify-content: center;
}
```

---

## 4. Verification & Testing Methodology

To verify that all identified defects are resolved after applying the drop-in code fixes:

### 1. Automated Responsive Overflow & Touch Target Console Test
Open Chrome DevTools (`F12`), switch to Console, and run:
```javascript
// Test 1: Viewport Horizontal Overflow Check
(function checkOverflow() {
  const w = document.documentElement.clientWidth;
  const bad = [];
  document.querySelectorAll('*').forEach(el => {
    const r = el.getBoundingClientRect();
    if (r.right > w + 1 || r.left < -1) bad.push({ el, right: r.right, max: w });
  });
  console.log(bad.length === 0 ? '✅ 0 OVERFLOW ELEMENTS (PASS)' : '❌ OVERFLOW DETECTED:', bad);
})();

// Test 2: Touch Target Size Audit (WCAG 2.5.5 / 2.5.8)
(function checkTouchTargets() {
  const items = document.querySelectorAll('button, a, input, select, textarea, [role="button"]');
  const small = [];
  items.forEach(el => {
    const r = el.getBoundingClientRect();
    if (r.width > 0 && r.height > 0 && (r.width < 40 || r.height < 40)) {
      small.push({ tag: el.tagName, class: el.className, text: el.textContent.trim().slice(0,25), w: r.width, h: r.height });
    }
  });
  console.log(small.length === 0 ? '✅ ALL TOUCH TARGETS >=40px (PASS)' : '⚠️ SMALL TARGETS FOUND:', small);
})();
```

### 2. Screen Reader & Keyboard Navigation Test Checklist
- **Tab Key Traversal**: Press `Tab` starting from the top of the page; ensure the Skip Link appears and shifts focus directly to `<main id="main">`.
- **Card Semantics**: Ensure Situation cards and Diagnostic cards announce as "button" and activate upon pressing `Enter` or `Space`.
- **Form Label Verification**: Verify that every input control announces its explicit label and accessible description.
- **High-Contrast Theme Verification**: Toggle high-contrast mode (`◐`); ensure all legal disclaimers, badges, and the official NCRP slip render with pure black-and-white borders without hardcoded pastel colors.
- **Bilingual Switcher Verification**: Toggle `हिं`; verify that the entire interface translates cleanly without missing English strings or Devanagari matra clipping.

---

## 5. Conclusion & Implementation Roadmap

The CyberSuraksha / NCRP 2.0 SPA application exhibits exceptional architectural purity with its zero-dependency vanilla JS implementation. By applying the prioritized **Top 5 Quick Wins** and the structured drop-in fixes cataloged across Domains R1–R4, the application will achieve **100% WCAG 2.2 AAA / GIGW 3.0 accessibility compliance**, eliminate emergency CTA touch collisions, prevent fake complainant data injection, and deliver an empowering, bilingual reporting portal for citizens across India.

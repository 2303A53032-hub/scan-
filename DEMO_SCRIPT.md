# Community Scam Map — 3-Minute Live Demo Script

This script guides presenters and judges through a comprehensive walkthrough of the **Community Scam Map** system, demonstrating real scam detection, adversarial defense, privacy protection, and cryptographic licensing.

---

## ⏱️ Minute 0:00 – 0:30: The Problem & The Solution

**Speaker:**
> *"Every day in India, citizens lose life savings to digital arrest scams, fake KYC update links, and WhatsApp investment groups. When a victim is targeted, there is no shared, privacy-safe way for their neighbors to check: 'Has anyone reported this number or link?'
> 
> We built **Community Scam Map**: a privacy-first platform that lets citizens check and report suspicious identifiers in under 60 seconds, with an explainable trust engine that rejects fake reports and report-bombing, combined with cryptographic licensing for banks and enterprise deployments."*

**Action on Screen:**
1. Open Home Page (`http://localhost:5173/`).
2. Show the clean, trustworthy cybersecurity interface.
3. Highlight the 3 prominent primary actions: **Check a Number**, **Report a Scam**, and **View Scam Map**.

---

## ⏱️ Minute 0:30 – 1:15: Public Lookup & Privacy Masking

**Speaker:**
> *"Let's test an incoming suspicious call Ravi's father received: `+91 98765 43210`. The user can type it with spaces, dashes, or leading zeroes — our normalization treats them as the same identifier."*

**Action on Screen:**
1. Click **Check Identifier** (`/lookup`).
2. Type `09876543210` and click **Check Now**.
3. **Show Verdict:** Display says `KNOWN_SCAM` with high confidence.
4. Highlight that 7 distinct citizens reported this across multiple IP prefixes, showing corroborated evidence (dates, channels) **without ever revealing private notes, reporter names, or IPs**.
5. Now search an unreported number: `+91 99999 00000` → Returns `NO_REPORTS` with cautious safety advice.
6. Now test an official helpline: `1930` or `Fresh Bakes` (`+91 80 4100 2233`) → Returns `VERIFIED_BUSINESS`. Even if 40 rivals report-bomb it, our trust engine protects verified businesses!

---

## ⏱️ Minute 1:15 – 1:45: 60-Second Anonymous Report & Elderly Mode

**Speaker:**
> *"Now let's submit a new report. No login is needed. No phone number or GPS is tracked — area PIN code only."*

**Action on Screen:**
1. Click **Report Scam** (`/report`).
2. Select Category (e.g. `Digital arrest video call 👮`).
3. Enter Number: `+91 99000 12345`, PIN: `560102`.
4. Click **Submit Anonymous Report** → Instant success response in < 15 seconds.
5. Click **👓 Elderly Mode Toggle** in top bar:
   - Text size expands to 21px+
   - Buttons enlarge to 60px touch targets
   - High contrast borders activate
6. Click **Language Selector** → Switch to **हिंदी (Hindi)** or **తెలుగు (Telugu)** to demonstrate full native localization.

---

## ⏱️ Minute 1:45 – 2:15: Interactive Map & Surge Alerts

**Speaker:**
> *"Let's see what is happening in our neighborhood. The Scam Map plots area clusters from Appendix B coordinates — never personal home locations."*

**Action on Screen:**
1. Click **Scam Map** (`/map`).
2. Show Leaflet map with colored clusters:
   - 🔴 **CRIT (Red):** HSR Layout (digital arrest surge)
   - 🟠 **HIGH (Orange):** Indiranagar
3. Click a pin to show the popup: report count, top scam type, severity. Toggle to **List View** for accessibility.
4. Click **Area Alerts** (`/alerts`):
   - Show the **SPIKE Alert**: Reports in last 24h doubled the 7-day baseline and reached ≥ 5.
   - Show the **NEW_TYPE Alert**: First occurrence of a novel scam category in 30 days.

---

## ⏱️ Minute 2:15 – 2:40: Business Dispute Protection

**Speaker:**
> *"What if a bakery or legitimate clinic is mistakenly reported? We provide an enterprise dispute mechanism."*

**Action on Screen:**
1. Click **Business Dispute** (`/dispute`).
2. Enter business name: `Fresh Bakes Indiranagar`, Number: `+91 80 4100 2233`.
3. Submit dispute → Status is immediately `under_review`.
4. Demonstrate Rule: While under review, lookup returns `challenged: true` and is **guaranteed NEVER to be marked KNOWN_SCAM**.

---

## ⏱️ Minute 2:40 – 3:00: Live Signed Licensing System

**Speaker:**
> *"Finally, our signed software licensing system built with PyNaCl Ed25519 cryptography. If a client's prepaid license expires, the system locks gracefully without deleting any data, and restores instantly upon hot renewal."*

**Action on Screen:**
1. Open **License** (`/license`).
2. Show current active license: `Demo Bank (Valid & Verified)`.
3. Click **Step 2 & 3: Force Expire License**.
   - Immediate red banner appears: `Software License Expired`.
   - Go to **Lookup** or **Map** → Protected endpoints return HTTP 403 `{"error": "LICENSE_EXPIRED"}`.
   - Notice `/health` and `/license` continue to respond!
4. Click **Step 4: Hot-Renew License**.
   - 1-Year renewed cryptographic license is uploaded.
   - Banner disappears immediately! All operations restored with **zero server restart** and all existing reports 100% intact!

**Speaker:**
> *"Community Scam Map delivers full-stack safety, privacy, explainable trust, and enterprise resilience. Thank you!"*

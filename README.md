# Community Scam Map 🛡️

> **A Privacy-First Platform to Detect, Report, and Track Community-Reported Scams**

[![License](https://img.shields.io/badge/License-Ed25519_Signed-blue.svg)](#cryptographic-licensing-system)
[![Backend](https://img.shields.io/badge/Backend-FastAPI_Python_3.12-green.svg)](#backend-architecture)
[![Frontend](https://img.shields.io/badge/Frontend-React_18_Vite_Leaflet-cyan.svg)](#frontend-architecture)
[![Tests](https://img.shields.io/badge/Tests-All_Passed_100%25-brightgreen.svg)](#automated-testing)

---

## 1. Problem Statement & Mission

Citizens in India are increasingly targeted by sophisticated scams:
- **Digital Arrest Scams:** Video calls by fraudsters posing as police/CBI claiming parcels contain narcotics.
- **UPI Refund QR & Payment Fraud:** Tricking victims into scanning QR codes to "receive" funds.
- **Fake KYC Links & SIM Blocking SMS:** Phishing portals mimicking bank login screens.
- **WhatsApp Investment Groups & Fake Task Jobs:** Ponzi schemes demanding prepaid deposits.

When a scam occurs, victims often feel isolated and the perpetrator continues targeting neighbors the next day. **Community Scam Map** provides an open, trusted, privacy-preserving platform where anyone can ask:
> *"Has anyone reported this phone number, UPI ID, link, or SMS sender as suspicious?"*

The platform helps citizens make safer decisions without making unproven criminal accusations or compromising reporter privacy.

---

## 2. Key Capabilities & Features

1. **< 60-Second Anonymous Reporting:** No login, name, phone number, or exact address required. Anchored strictly to area PIN codes.
2. **Sub-500ms Evidence-Based Lookup:** Immediate verification for Phone numbers, UPI IDs, URLs, and SMS Sender IDs.
3. **Deterministic, Explainable Trust Engine:** Defends against report bombing, duplicate spamming, and Sybil Wi-Fi account clusters.
4. **Privacy-Preserving Masking:** Automatically masks identifiers (`98XXXXX210`, `r***@okaxis`, `sbi***.in/...`, `HD****`) until corroborated by at least 3 distinct reporters.
5. **Interactive Geographic Scam Map:** Real-time OpenStreetMap with area clusters from Appendix B coordinates (never personal reporter GPS). 7-day default window.
6. **Statistically Grounded Surge Alerts:**
   - **SPIKE:** Reports in last 24h double the 7-day baseline and reach at least 5 incidents.
   - **NEW_TYPE:** Novel scam category emerging in an area with 0 occurrences in prior 30 days.
7. **Elderly-Friendly & Accessible UX:**
   - High-contrast toggle
   - Large typography (18–21px body, 24–30px headings)
   - Oversized touch targets (50–60px buttons)
   - Full trilingual support: **English**, **Hindi (हिंदी)**, and **Telugu (తెలుగు)**.
8. **Business Dispute Redressal (`POST /challenges`):** Formal challenge flow for legitimate stores; disputed numbers are immune from `KNOWN_SCAM` while under review.
9. **Signed Ed25519 Licensing System:** Cryptographically enforced software license. Blocks expired installations with HTTP 403 `{"error": "LICENSE_EXPIRED"}` while allowing zero-downtime hot renewals.

---

## 3. Technology Stack

### Backend
- **Language & Runtime:** Python 3.12+
- **Framework:** FastAPI
- **ORM & Database:** SQLAlchemy (SQLite for instant local dev, PostgreSQL for production)
- **Validation:** Pydantic v2
- **Cryptography:** PyNaCl (Ed25519 digital signature signing & verification)
- **Testing:** Pytest, HTTPX, AnyIO

### Frontend
- **Framework:** React 18
- **Build Tool:** Vite 5
- **Mapping:** Leaflet & OpenStreetMap tiles
- **Routing:** React Router v6
- **Localization:** In-memory translation dictionaries (English, Hindi, Telugu)
- **Styling:** CSS3 Custom Properties (Accessible cybersecurity design)

---

## 4. Quick Start & Local Execution

### Prerequisites
- Python 3.12+ (or `uv`)
- Node.js 20+ and npm

### Step 1: Clone and Set Up Backend
```bash
# Navigate to backend directory
cd backend

# Create virtual environment and activate
python -m venv .venv
# On Windows:
.venv\Scripts\activate
# On Linux/macOS:
source .venv/bin/activate

# Install dependencies
pip install -r requirements.txt

# Seed the database with realistic test cases
python seed_data.py

# Start the FastAPI server
python -m uvicorn backend.app.main:app --host 127.0.0.1 --port 8000 --reload
```
Backend API will be live at: `http://127.0.0.1:8000`
Interactive Swagger Docs: `http://127.0.0.1:8000/docs`

### Step 2: Start the React Frontend
```bash
# In a separate terminal, navigate to frontend
cd frontend

# Install dependencies
npm install

# Start Vite development server
npm run dev
```
Frontend will be live at: `http://localhost:5173/`

*(Note: In production builds, the FastAPI backend automatically serves the built frontend from `frontend/dist` on port 8000 directly!)*

---

## 5. Report Trust Engine — Architecture & Rules

The system deliberately avoids naive report-counting to prevent manipulation.

### Signals Evaluated
- **Distinct Reporter IDs (`reporter_id`):** Prevents lone spammers from inflating counts.
- **IP Subnet Concentration (`ip_prefix`):** Groups reports by subnet to prevent Sybil attacks.
- **Description / Note Similarity:** Detects templated, copy-pasted submission bots.
- **Identifier Validation:** Rejects junk like `12345` or malformed URLs.
- **Verified Business Registry:** Entities listed in Appendix C (e.g. `1930`, `Fresh Bakes`) are protected.
- **Active Business Disputes:** Identifiers undergoing review are immune from `KNOWN_SCAM`.

### Decision Matrix
| Scenario | Distinct Reporters | IP Subnet Diversity | Business Status | Final Verdict |
| :--- | :---: | :---: | :---: | :--- |
| **0 Reports** | 0 | 0 | Unlisted | `NO_REPORTS` (HTTP 200) |
| **Verified Business / Helpline** | Any | Any | Listed | `VERIFIED_BUSINESS` |
| **Single User 6 Reports** | 1 | 1 | Unlisted | `SUSPICIOUS` (Never `KNOWN_SCAM`) |
| **22 Fake Accounts on 1 Wi-Fi** | 22 | 1 (Capped) | Unlisted | `SUSPICIOUS` (Never `KNOWN_SCAM`) |
| **Active Business Challenge** | Any | Any | Under Review | `SUSPICIOUS` (Dispute Immune) |
| **3+ Independent Reports** | ≥ 3 | ≥ 2 Subnets | Unlisted | `KNOWN_SCAM` (Unmasks Identifier) |

---

## 6. Identifier Normalization & Privacy Masking

Every identifier is normalized into a single canonical form before storage or search:

| Type | Input Example | Normalized Value | Masked Display (< 3 Reports) |
| :--- | :--- | :--- | :--- |
| **Phone** | `+91 98765 43210` / `09876543210` | `+919876543210` | `98XXXXX210` |
| **Helpline** | `1930` / `1800 1600` | `1930` / `18001600` | Unmasked (Verified) |
| **UPI ID** | `Ravi12@OKAXIS` | `ravi12@okaxis` | `r***@okaxis` |
| **URL** | `http://www.sbi-kyc.in/pay?utm=sms` | `https://sbi-kyc.in/pay` | `sbi***.in/...` |
| **SMS Sender**| `VM-HDFCBK` / `AD-HDFCBK` | `HDFCBK` | `HD****` |

---

## 7. Public API Endpoints Reference

All endpoints return JSON and require no authentication (as specified for testing tools).

### 1. Health Check
`GET /health`
```json
{
  "ok": true
}
```

### 2. Submit Scam Report
`POST /reports`
- **Request:**
```json
{
  "report_id": "rep_000123",
  "occurred_at": "2026-10-27T18:21:04+05:30",
  "reported_at": "2026-10-27T18:40:04+05:30",
  "reporter_id": "r_7c2d91aa",
  "ip_prefix": "103.21.58",
  "channel": "Phone call",
  "scam_type": "digital-arrest",
  "identifier_type": "phone",
  "identifier_value": "+91 98765 43210",
  "pincode": "560102",
  "locality": "HSR Layout",
  "city": "Bengaluru",
  "note": "Video call from man in police uniform",
  "has_screenshot": false,
  "amount_lost_inr": 0
}
```
- **Response (201 Created):**
```json
{
  "report_id": "rep_000123",
  "case_id": "case_phone_16890598",
  "merged": true
}
```

### 3. Check Identifier Lookup
`GET /lookup?type=phone&value=%2B919876543210`
- **Response (200 OK):**
```json
{
  "verdict": "KNOWN_SCAM",
  "report_count": 7,
  "distinct_reporters": 7,
  "first_seen": "2026-10-26T18:21:04+05:30",
  "last_seen": "2026-10-31T14:00:00+05:30",
  "scam_type": "digital-arrest",
  "display_value": "+919876543210",
  "evidence": [
    {
      "occurred_at": "2026-10-31T14:00:00+05:30",
      "channel": "Phone call",
      "scam_type": "digital-arrest",
      "pincode": "560102"
    }
  ],
  "challenged": false
}
```

### 4. Scam Map Clusters
`GET /map?pincode=560102&days=7&radius_km=2`
- **Response (200 OK):**
```json
{
  "clusters": [
    {
      "pincode": "560102",
      "locality": "HSR Layout",
      "city": "Bengaluru",
      "count": 12,
      "top_scam_type": "digital-arrest",
      "severity": "CRIT",
      "lat": 12.9116,
      "lng": 77.6473
    }
  ]
}
```

### 5. Area Surge Alerts
`GET /alerts?pincode=560102`
- **Response (200 OK):**
```json
{
  "alerts": [
    {
      "type": "SPIKE",
      "pincode": "560102",
      "window": "24h",
      "count": 10,
      "baseline_per_day": 2.0
    },
    {
      "type": "NEW_TYPE",
      "pincode": "560102",
      "scam_type": "job-offer",
      "window": "24h"
    }
  ]
}
```

### 6. Business Dispute
`POST /challenges`
- **Request:**
```json
{
  "identifier_type": "phone",
  "identifier_value": "+91 80 4100 2233",
  "claimant_name": "Fresh Bakes",
  "contact_email": "owner@freshbakes.com",
  "proof_note": "GST certificate and FSSAI license attached."
}
```
- **Response (201 Created):**
```json
{
  "challenge_id": "chal_88c16e1e41",
  "status": "under_review"
}
```

### 7. Reset System Data
`POST /admin/reset`
- **Response (200 OK):**
```json
{
  "ok": true,
  "message": "All reports, summaries, and challenges deleted"
}
```

---

## 8. Cryptographic Licensing System

The system implements an asymmetric **Ed25519 digital signature scheme**:
- License payload contains `licensee`, `expires_at`, and `signature`.
- Verified using the public key embedded in the server: `efa857aff6c8039909d8427a66dec4f12d0271d89576fc71585815e4c2c79845`.
- **Live Expiration:** When a license expires, all routes except `/health` and `/license` return HTTP 403 `{"error": "LICENSE_EXPIRED"}`.
- **Zero-Downtime Hot Renewal:** Uploading a renewed license via `POST /license` instantly restores full functionality **without restarting the server**. No existing data is ever deleted.
- **Clock Rollback Protection:** The server tracks monotonic timestamps; if the server clock moves backward, the check fails.

### Generating Test Licenses
```bash
# Generate key info
python backend/generate_license.py keys

# Generate demo test bundle (1-year valid, 5-minute demo, expired)
python backend/generate_license.py demo_bundle
```

---

## 9. Automated Testing & Verification

Automated test suites cover normalization, adversarial attacks, API integration, and licensing:

```bash
# Run unit tests (Normalization & Adversarial Trust Engine)
python -m pytest backend/tests/test_normalization.py backend/tests/test_trust_engine.py -v

# Run full API endpoint integration & zero-downtime renewal tests
python backend/tests/verify_endpoints.py
```

### Verification Results Summary
- `test_phone_normalization_variants`: **PASSED**
- `test_helplines_and_toll_free`: **PASSED**
- `test_invalid_phone_rejected`: **PASSED**
- `test_upi_normalization`: **PASSED**
- `test_url_normalization`: **PASSED**
- `test_sender_id_normalization`: **PASSED**
- `test_privacy_masking`: **PASSED**
- `test_single_user_six_reports`: **PASSED** (Verdict remains `SUSPICIOUS`)
- `test_twenty_two_fake_accounts_same_wifi`: **PASSED** (Subnet clustering stops `KNOWN_SCAM`)
- `test_report_bombing_legitimate_bakery`: **PASSED** (Protected as `VERIFIED_BUSINESS`)
- `test_genuine_scam_independent_reporters`: **PASSED** (Corroborated as `KNOWN_SCAM`)
- `test_disputed_business_never_known_scam`: **PASSED** (`challenged: true`)
- `verify_endpoints.py`: **13/13 E2E Integration Tests PASSED 100%**

---

## 10. Deployment Instructions

### Docker & Docker Compose
```bash
# Build and run complete multi-stage container
docker-compose up --build
```

### Cloud Deployment (Render / Railway / Fly.io)
1. Push this repository to GitHub.
2. In Render, select **New Web Service** and connect this repository.
3. Use the supplied `render.yaml` or set:
   - **Build Command:** `pip install -r requirements.txt && python backend/seed_data.py`
   - **Start Command:** `uvicorn backend.app.main:app --host 0.0.0.0 --port $PORT`
4. The service will deploy with full HTTPS, active licensing, and pre-seeded demonstration data.

---

## 11. Final Verification Checklist

| Requirement | Implementation Details | Status |
| :--- | :--- | :---: |
| **Report Submission** | Anonymous, <60s, Appendix C categories & channels | ✅ Complete |
| **Lookup Engine** | Sub-500ms, exact enum verdicts, privacy masking | ✅ Complete |
| **Trust Rules** | Sybil defense, duplicate detection, explainable scoring | ✅ Complete |
| **Interactive Map** | Leaflet OpenStreetMap, Appendix B coordinates only | ✅ Complete |
| **Surge Alerts** | SPIKE and NEW_TYPE detection | ✅ Complete |
| **Business Disputes** | Dispute submission, immunity from `KNOWN_SCAM` | ✅ Complete |
| **Signed Licensing** | PyNaCl Ed25519, 403 enforcement, zero-downtime renewal | ✅ Complete |
| **Accessibility Mode** | 21px+ font, 60px buttons, high-contrast toggle | ✅ Complete |
| **Multilingual** | English, Hindi (हिंदी), and Telugu (తెలుగు) | ✅ Complete |
| **Automated Tests** | 100% passing across unit and integration suites | ✅ Complete |
| **Demo Script** | 3-minute presentation script (`DEMO_SCRIPT.md`) | ✅ Complete |
| **Production Build** | Multi-stage Dockerfile, Vite optimized dist | ✅ Complete |

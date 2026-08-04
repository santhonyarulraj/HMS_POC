# 04 — Mandatory Fields and Validation Rules

All rules below are transcribed directly from each feature's "Data Specifications" (UI Schema) and "Validations" tables in the source FRDs — including the exact quoted messages, which should be used verbatim as Playwright assertion targets.

---

## A. Login Module

### A.1 Mandatory Fields by Screen

| Screen (Feature) | Field | Mandatory? | Default |
|---|---|---|---|
| Login (HC-USR-001) | Username | M | empty |
| Login (HC-USR-001) | Password | M | empty |
| Change Own Password (HC-USR-010) | Current Password | M | empty |
| Change Own Password (HC-USR-010) | New Password | M | empty |
| Change Own Password (HC-USR-010) | Confirm New Password | M | empty |
| Admin Force Password Reset (HC-USR-011) | Force change at next login (checkbox) | M | checked |
| Admin Force Password Reset (HC-USR-011) | Temporary Password | O | empty |
| Request Reset Code (HC-USR-012) | Username or Email | M | empty |
| Reset Password with Code (HC-USR-013) | Reset Code (6 digits) | M | empty |
| Reset Password with Code (HC-USR-013) | New Password | M | empty |
| Reset Password with Code (HC-USR-013) | Confirm New Password | M | empty |
| 2FA Setup (HC-USR-014) | Confirmation Code (6 digits) | M | empty |
| 2FA Verify at Login (HC-USR-015) | Authentication Code (6 digits) | M | empty |

### A.2 Validation Rules (Field, Condition, Exact Message)

| Feature | Field | Condition | Validation Message (verbatim) |
|---|---|---|---|
| HC-USR-001 | Username | Not empty | `"Required."` |
| HC-USR-001 | Password | Not empty | `"Required."` |
| HC-USR-001 | Credentials | Must match an existing active account | `"Invalid username or password."` |
| HC-USR-001 | Account status | Must be active | `"Account deactivated. Please contact your administrator."` |
| HC-USR-001 | (rate limit) | ≤5 attempts/minute | `"Too many attempts. Please try again in a minute."` |
| HC-USR-010 | Current Password | Must match stored credential | `"Current password is incorrect."` |
| HC-USR-010 | New Password | Meets complexity policy | `"Password must be at least 8 characters with an uppercase letter, a number, and a special character."` |
| HC-USR-010 | Confirm New Password | Must match New Password | `"Passwords do not match."` |
| HC-USR-011 | Temporary Password | Meets complexity policy (when provided) | `"Password must be at least 8 characters with an uppercase letter, a number, and a special character."` |
| HC-USR-012 | Username or Email | Not empty | `"Required."` |
| HC-USR-013 | Reset Code | Valid, unexpired, unused 6-digit code | `"Invalid or expired code."` |
| HC-USR-013 | New Password | Meets complexity policy | `"Password must be at least 8 characters with an uppercase letter, a number, and a special character."` |
| HC-USR-013 | Confirm New Password | Must match New Password | `"Passwords do not match."` |
| HC-USR-014 | Confirmation Code | Current valid 6-digit code from paired app | `"Invalid code. Please try again."` |
| HC-USR-015 | Authentication Code | Current valid TOTP or unused backup code | `"Invalid code. Please try again."` |
| HC-USR-015 | Attempts | ≤5 consecutive failures | `"Too many failed attempts. Your account is locked for 15 minutes."` |

### A.3 Success Messages (verbatim, for assertion)

- Login: no message — user lands on the dashboard.
- Logout: no message — user lands on the Login screen.
- Change Own Password: `"Password changed successfully."`
- Admin Force Password Reset: `"Password reset applied. The user must change their password at next login."`
- Request Reset Code: `"If an account exists for the details entered, a reset code has been emailed."`
- Reset Password with Code: `"Password reset successfully. Please log in with your new password."`
- 2FA Setup: `"Two-factor authentication enabled successfully."`

---

## B. Patient Registration Module

### B.1 Mandatory Fields by Flow

| Flow | Field | Mandatory? | Default |
|---|---|---|---|
| **Full Wizard (HC-PAT-001)** | First Name | M | empty |
| | Last Name | M | empty |
| | Date of Birth | O | empty |
| | Gender | O | Male |
| | Patient Type | O | OPD |
| | Phone | M | empty |
| | Category | O | General |
| | Blood Group | O | empty |
| | Aadhaar Number | O | empty |
| | ABHA ID | O | empty |
| | Consent toggles (×4) | O | Off |
| | Is MLC | O | Off |
| | FIR Number (if MLC on) | O | empty |
| **Quick Registration (HC-PAT-002)** | First Name | M | empty |
| | Last Name | M | empty |
| | Phone | M | empty |
| | Date of Birth | O | empty |
| | Gender | O | empty |
| **Emergency Registration (HC-PAT-003)** | Patient Name | M ("Unknown" accepted) | empty |
| | Phone | O | empty |
| | Chief Complaint | M | empty |
| | Gender / Age / DOB | O | empty |
| | Emergency Type / Mode of Arrival | O | empty |
| | Triage Priority | O | P2 |
| | Is MLC | O | Off |
| **ABHA OTP Generation (HC-PAT-022)** | ABDM-registered Mobile | M | empty |
| **ABHA OTP Verification (HC-PAT-023)** | OTP (6 digits) | M | empty |
| **ABHA Search (HC-PAT-024)** | ABHA ID or Address | M | empty |

### B.2 Validation Rules (Field, Condition, Exact Message)

| Feature | Field | Condition | Validation Message (verbatim) |
|---|---|---|---|
| HC-PAT-001 | First Name / Last Name | Not empty | `"Required."` |
| HC-PAT-001 | Phone | Numeric, 10 digits | `"Enter a valid 10-digit phone number."` |
| HC-PAT-001 | Email | Valid format (when provided) | `"Enter a valid email address."` |
| HC-PAT-001 | PIN Code | Numeric, 6 digits (when provided) | `"Enter a valid 6-digit PIN code."` |
| HC-PAT-001 | Aadhaar Number | Numeric, 12 digits (when provided) | `"Enter a valid 12-digit Aadhaar number."` |
| HC-PAT-001 | Phone (duplicate) | Existing patient with same phone in facility → **warning, not block** | `"A patient with this phone number already exists: [Name, UHID]."` |
| HC-PAT-002 | First / Last Name / Phone | Not empty | `"Required."` |
| HC-PAT-002 | Phone | Numeric, 10 digits | `"Enter a valid 10-digit phone number."` |
| HC-PAT-002 | Phone (duplicate) | Must not match an existing patient in facility → **blocks creation** | `"A patient with this phone already exists: [Name] ([UHID]). Please select them instead."` |
| HC-PAT-002 | (facility) | Acting user must have a facility assigned | `"Your account has no facility assigned. Contact your administrator."` |
| HC-PAT-003 | Patient Name | Not empty ("Unknown" permitted) | `"Required."` |
| HC-PAT-003 | Chief Complaint | Not empty | `"Required."` |
| HC-PAT-003 | (facility) | Acting user must have a facility assigned | `"Your account has no facility assigned. Contact your administrator."` |
| HC-PAT-017 | Phone | Checked on field-blur during Step 2 | (see HC-PAT-001 duplicate message above — same mechanism) |
| HC-PAT-022 | Mobile | Numeric, 10 digits | `"Enter a valid 10-digit mobile number."` |
| HC-PAT-023 | OTP | Exactly 6 digits, valid for the transaction | `"OTP verification failed. Please try again."` |
| HC-PAT-024 | Query (ABHA ID/Address) | Not empty | `"Required."` |
| HC-PAT-024 | (no match) | — | `"No ABHA record found for this query."` |

### B.3 Success Messages (verbatim, for assertion)

- Full Wizard: `"Patient registered successfully — UHID: [PATxxxxxxxx]."`
- Quick Registration: `"Patient [name] registered — UHID: [id]."`
- Emergency Registration: `"Emergency patient registered."`
- ABHA OTP Generation: `"OTP sent to the patient's ABHA-linked mobile."`
- ABHA OTP Verification: `"ABHA verified. Details filled automatically."`

### B.4 Error Messages Not Field-Specific (System/Load Failures)

- `"Could not send ABHA OTP. Please verify the mobile number and try again."` (HC-PAT-022)
- `"Could not load QR code."` / `"QR not available."` (HC-PAT-005)

---

## C. Note on Format Ambiguities

Several "format" validations (email, PIN code, Aadhaar) specify a digit count or "valid format" but do not spell out the exact regex/character-class rules in the FRD text (e.g., is a `+91` prefix on phone accepted? are hyphens/spaces in Aadhaar tolerated before validation?). These are captured as open items in `07_Client_Questions.md` rather than assumed.

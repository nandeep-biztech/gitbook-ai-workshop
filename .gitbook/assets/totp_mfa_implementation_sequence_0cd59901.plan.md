---
name: TOTP MFA Implementation Sequence
overview: Implementation plan for 13 TOTP MFA tickets, sequenced by dependencies. Foundation first (core TOTP services), then enrollment flows, login flows, account settings, and admin management.
todos:
  - id: pp-1544
    content: "PP-1544: Implement FE Web Server MFA/TOTP Core Logic - TOTP generation/validation, QR generation, backup codes, MFA state management"
    status: pending
  - id: pp-1545
    content: "PP-1545: Implement TOTP MFA Selection screen for Customer Portal"
    status: pending
  - id: pp-1546
    content: "PP-1546: Implement TOTP MFA Selection screen for Admin/Creative Portal"
    status: pending
  - id: pp-1547
    content: "PP-1547: Implement TOTP Enrollment flow for Admin/Creative Portal"
    status: pending
  - id: pp-1548
    content: "PP-1548: Implement TOTP Enrollment flow for Customer Portal"
    status: pending
  - id: pp-1551
    content: "PP-1551: Implement TOTP and Backup Code login flows for Admin/Creative Portal"
    status: pending
  - id: pp-1552
    content: "PP-1552: Implement TOTP and Backup Code login flows for Customer Portal"
    status: pending
  - id: pp-1549
    content: "PP-1549: Implement MFA Challenge Routing After Login for Customer Portal"
    status: pending
  - id: pp-1550
    content: "PP-1550: Implement MFA Challenge Routing After Login for Admin/Creative Portal"
    status: pending
  - id: pp-1553
    content: "PP-1553: Implement Customer Account Settings MFA management"
    status: pending
  - id: pp-1554
    content: "PP-1554: Implement User Account Settings MFA management"
    status: pending
  - id: pp-1555
    content: "PP-1555: Implement Admin Customer Management MFA Status & Reset"
    status: pending
  - id: pp-1556
    content: "PP-1556: Implement Admin User Management MFA Status & Reset"
    status: pending
---

# TOTP MFA Implementation Sequence

## Implementation Order Justification

The tickets must be implemented in this order because:

1. **PP-1544** (Foundation) - Provides all core TOTP services (generation, validation, QR, backup codes, MFA state) that every other ticket depends on
2. **PP-1545/PP-1546** (Entry Points) - MFA selection screens that route to enrollment; depend on PP-1544's state management
3. **PP-1547/PP-1548** (Enrollment) - TOTP enrollment flows; depend on PP-1544's TOTP/QR/backup services and PP-1545/PP-1546 for entry
4. **PP-1549/PP-1550** (Routing) - Post-login routing logic; depend on PP-1544's state and PP-1551/PP-1552 login flows
5. **PP-1551/PP-1552** (Login Flows) - TOTP/backup code login; depend on PP-1544's validation services
6. **PP-1553/PP-1554** (Account Settings) - User-facing MFA management; depend on enrollment flows and state management
7. **PP-1555/PP-1556** (Admin Management) - Admin MFA management UI; depend on all previous work for complete MFA lifecycle

---

## 1. PP-1544 — FE Web Server MFA/TOTP Core Logic (Shared OMS & Client Portal)

**Why This Ticket Is Implemented At This Stage**

- Foundation for all TOTP functionality. Every other ticket depends on TOTP generation, validation, QR generation, backup codes, and MFA state management provided here.

**Summary**

- Implement shared backend services for TOTP (RFC 6238), QR code generation, backup code lifecycle, and unified MFA state orchestration in the FE webserver.

**Requirements**

- TOTP generation/validation with ±1 time step window (~90s effective)
- Server-side QR code generation (otpauth:// URI) - never expose plaintext secret in browser
- Backup code generation, hashing, validation, mark-as-used, remaining count
- MFA state management: DISABLED, SMS, TOTP_AUTHENTICATOR_APP, RESETTING
- Routing helpers: "Should show MFA enrollment selection?", "Current MFA method and backup code count"
- Integration with OMS/Client Portal auth APIs using requestNewTOTPkey=true header

**Dependencies**

- **Backend APIs (PP-1344 - COMPLETED)**:
- OMS API 4.1 User Authentication (Rev 3.16) - Returns `TotpSecretKey` on successful authentication
- Client Portal API 4.1 Organization Member Authentication (Rev 1.26) - Returns `TotpSecretKey` on successful authentication
- Header parameter: `requestNewTOTPkey=true` for generating/regenerating secret (requires authentication)
- Secret format: 20-character alphanumeric (A-Z, 0-9) - 160 bits, encrypted with AES in BE database
- Secret is ONLY returned on successful authentication, NOT in lookup APIs (security requirement)
- Existing MFA infrastructure (SMS flows)

**Backend Implementation Details (from PP-1344)**

- **Secret Management**: Backend generates 20-character alphanumeric secret (A-Z, 0-9), encrypted with AES, stored in database
- **Secret Retrieval**: Secret is returned in authentication response as `TotpSecretKey` property (only on successful auth)
- **Secret Generation**: Use `requestNewTOTPkey=true` header with authentication API to generate/regenerate secret
- **Backup Codes**: 6 random 10-character codes per user (from PP-1344 requirements)
- **Backup Code Storage**: Must be hashed (one-way) and stored in FE webserver (not in BE per PP-1344)

**Security Requirements (from PP-1344 Comments)**

- **TOTP Secret Key**: Must be contained at Web server (Next.js), NEVER in browser
- **QR Code Generation**: MUST be server-side (Next.js), never client-side JavaScript
- Server-side generation keeps secret key contained on server
- Client-side generation risks secret exposure in browser source code or memory
- **Manual Secret Entry**: NOT recommended (violates security - secret would be visible)
- **TOTP Code Generation**: Happens in FE Web Server (Next.js) at time of user submission to eliminate latency
- **Browser Role**: Limited to displaying server-generated QR code and accepting 6-digit code from user

**Library Decisions**

- **TOTP Library**: `otpauth` - Recommended in PP-1344, fully TypeScript, RFC 6238 compliant, better secret management with Secret class, built-in URI generation
- **QR Code Library**: `qrcode` (https://www.npmjs.com/package/qrcode) - Well-maintained, supports Node.js and browser, TypeScript types available
- **CRITICAL**: QR code generation MUST be server-side (Next.js), never in browser to prevent secret exposure

**Service Location Decisions**

- **API Endpoints**: `packages/shared/api/mfa/totp/` - Server-side API route handlers for TOTP operations
- **Hooks/Services**: `packages/shared/services/mfa/totp/` - Client-side hooks and service functions for TOTP operations

**Affected Code Areas**

- `packages/shared/api/mfa/totp/` - API route handlers for TOTP operations (server-side)
- `packages/shared/api/mfa/backupCodes/` - API route handlers for backup code operations (server-side)
- `packages/shared/services/mfa/totp/` - Client-side hooks and service functions for TOTP operations
- `packages/shared/services/mfa/backupCodes/` - Client-side hooks and service functions for backup code operations
- `packages/shared/api/utils/mfa/` - MFA state helpers and utilities
- Session types (`packages/shared/@types/session.ts`) - Add TOTP-related session fields

**Implementation Steps**

1. Install libraries: `yarn add otpauth qrcode` and `yarn add -D @types/qrcode`
2. Create TOTP API endpoints in `packages/shared/api/mfa/totp/`:

- **Secret Retrieval**: Call BE authentication API with `requestNewTOTPkey=true` header to get secret (BE generates 20-char alphanumeric, FE receives)
- **TOTP Instance Creation**: Create TOTP instance in FE Web Server (Next.js) using `otpauth`:
  ```typescript
  import * as OTPAuth from "otpauth";
  const secret = new OTPAuth.Secret({ size: 20 }); // Or use secret from BE
  const totp = new OTPAuth.TOTP({
    algorithm: "SHA1",
    digits: 6,
    period: 30,
    secret: secret,
    issuer: "Proofed",
    label: userEmail
  });
  ```

- **TOTP Code Generation**: Generate TOTP codes in FE Web Server (Next.js) using `totp.generate()` - NOT in browser
- **TOTP Code Validation**: Validate codes using `totp.validate({ token, window: 1 })` with ±1 time step window
- **OTPAuth URI Generation**: Use `totp.toString()` to get otpauth:// URI (built-in, no helper needed)
- **QR Code Generation** (SERVER-SIDE ONLY):
- Use `QRCode.toBuffer(totp.toString())` for server-side generation
- Returns buffer or data URL (never expose secret in browser)
- Browser only displays the generated QR code image

3. Create backup code API endpoints in `packages/shared/api/mfa/backupCodes/`:

- `generateCodes()` - Generate 6 random 10-character codes
- `hashCode()` - Hash codes using one-way hash (bcrypt or similar)
- `validateAndMarkUsed()` - Validate code and mark as used (one-time use)
- `getRemainingCount()` - Return count of unused backup codes
- Store hashed codes in FE webserver (session or database)

4. Create MFA state helpers in `packages/shared/api/utils/mfa/`:

- `getMFAState(user)` - Determine current MFA state (DISABLED, SMS, TOTP_AUTHENTICATOR_APP, RESETTING)
- `shouldShowEnrollmentSelection(user)` - Check if MFA selection screen should be shown
- `getCurrentMFAMethod(user)` - Get current active MFA method

5. Create client-side hooks/services in `packages/shared/services/mfa/totp/`:

- Hooks for TOTP operations (useTOTPGenerate, useTOTPVerify, etc.)
- These call FE Web Server API endpoints (not BE directly)

6. Create client-side hooks/services in `packages/shared/services/mfa/backupCodes/`:

- Hooks for backup code operations (useBackupCodeGenerate, useBackupCodeVerify, etc.)

7. Create API route handlers: `/api/mfa/totp/generate`, `/api/mfa/totp/verify`, `/api/mfa/backup-codes/generate`, `/api/mfa/backup-codes/verify`
8. Update login API to retrieve `TotpSecretKey` from BE authentication response when available
9. Add session types for TOTP state (totpSecret, backupCodes, mfaMethod)
10. Implement "Remember this browser" logic in session cookie (browser-level, only after MFA validation)

**Testing Checklist**

- TOTP code validates within ±1 time step, rejects outside window
- QR code generated server-side only (Next.js), secret never in browser/DOM/network logs
- Secret retrieved from BE authentication API correctly (only on successful auth)
- `requestNewTOTPkey=true` header generates new secret and invalidates previous
- Backup codes: generate 6 codes (10 chars each) → show once → validate → mark used → count decrements
- Regeneration invalidates all prior backup codes
- MFA state helpers return correct state across OMS/Client Portal
- TOTP code generation happens in FE Web Server (Next.js), not browser
- "Remember this browser" only works after MFA validation

**Estimate**

- Effort: L (2.0 story points)
- Risk: Medium (new cryptographic functionality, security-critical)

---

## 2. PP-1545 — TOTP: MFA Selection (Customer Area)

**Why This Ticket Is Implemented At This Stage**

- Entry point for customer TOTP enrollment. Depends on PP-1544's MFA state management to determine when to show selection screen.

**Summary**

- Create "Choose your authentication method" screen for customers with TOTP (recommended) and SMS options.

**Requirements**

- Show when MFA state is DISABLED or RESETTING after primary auth
- Two options: "Authenticator App (TOTP)" (recommended, first) and "SMS" (second)
- TOTP option navigates to PP-1548 enrollment flow
- SMS option navigates to existing SMS enrollment flow
- Do not show if MFA Override = TRUE

**Dependencies**

- PP-1544 (MFA state helpers: shouldShowEnrollmentSelection)
- PP-1548 (TOTP enrollment flow - entry point)
- Existing SMS enrollment flow

**Clarifications Needed**

- Exact route path for this screen? (`/account/mfa/select` or `/mfa/select`?)
- Should this be a shared component with PP-1546 or separate implementations?

**Affected Code Areas**

- `apps/customer-portal/components/pages/mfa-selection/` - New page component
- `apps/customer-portal/pages/mfa/select.tsx` - Route handler
- `apps/customer-portal/api/enhancers/withUserProvided/` - MFA state check in getServerSideProps
- `packages/shared/components/molecules/MFASelectionCard/` - Shared selection card (if shared with PP-1546)

**Implementation Steps**

1. Create MFA selection page component with two option cards
2. Add route `/mfa/select` with getServerSideProps checking MFA state
3. Implement routing logic: TOTP → `/mfa/totp/enroll`, SMS → existing SMS flow
4. Style TOTP option as "Recommended" with green badge
5. Add keyboard navigation and accessibility (ARIA labels)
6. Integrate with PP-1544's shouldShowEnrollmentSelection helper

**Testing Checklist**

- New customer with no MFA → selection shown with TOTP pre-selected
- RESETTING/DISABLED state → selection shown
- MFA Override = TRUE → selection not shown
- TOTP option routes to enrollment flow
- SMS option routes to existing SMS flow
- Keyboard navigation works correctly

**Estimate**

- Effort: S (0.5 story points)
- Risk: Low

---

## 3. PP-1546 — TOTP: MFA Selection (User Area)

**Why This Ticket Is Implemented At This Stage**

- Entry point for user (Admin/Creative Portal) TOTP enrollment. Can be implemented in parallel with PP-1545 since they share logic but are separate portals.

**Summary**

- Create "Choose your authentication method" screen for users (Admin/Creative Portal) with TOTP (recommended) and SMS options.

**Requirements**

- Same as PP-1545 but for Admin/Creative Portal
- Show when MFA state is DISABLED or RESETTING after primary auth
- Two options: "Authenticator App (TOTP)" (recommended) and "SMS"
- TOTP option navigates to PP-1547 enrollment flow
- SMS option navigates to existing SMS enrollment flow
- Do not show if MFA Override = TRUE

**Dependencies**

- PP-1544 (MFA state helpers)
- PP-1547 (TOTP enrollment flow - entry point)
- Existing SMS enrollment flow
- Can share components with PP-1545

**Clarifications Needed**

- Should PP-1545 and PP-1546 share a component in `packages/shared/components/` or be separate?

**Affected Code Areas**

- `apps/creative-portal/components/pages/mfa-selection/` - New page component
- `apps/creative-portal/pages/mfa/select.tsx` - Route handler
- `apps/creative-portal/api/enhancers/withUserProvided/` - MFA state check
- `packages/shared/components/molecules/MFASelectionCard/` - Shared component (if shared)

**Implementation Steps**

1. Create MFA selection page component (reuse shared component if possible)
2. Add route `/mfa/select` with getServerSideProps
3. Implement routing: TOTP → `/mfa/totp/enroll`, SMS → existing SMS flow
4. Style and accessibility same as PP-1545
5. Integrate with PP-1544's shouldShowEnrollmentSelection helper

**Testing Checklist**

- Same as PP-1545 but for Admin/Creative Portal context
- Correct routing to PP-1547 enrollment flow

**Estimate**

- Effort: S (0.5 story points - can reuse components from PP-1545)
- Risk: Low

---

## 4. PP-1547 — TOTP: Enrollment (Admin/Creative Area)

**Why This Ticket Is Implemented At This Stage**

- Enrollment flow for users. Depends on PP-1544's TOTP/QR/backup code services and PP-1546 for entry point.

**Summary**

- Implement TOTP enrollment screen with QR code, manual secret entry, code verification, and backup codes modal for Admin/Creative Portal.

**Requirements**

- Entry points: From PP-1546 MFA selection or from Account Settings "Reset authenticator app"
- Three-step enrollment: Get App, Scan QR Code, Enter Code
- Server-side QR generation (never expose secret in browser)
- Manual secret key display (monospace, red pill style)
- 6-digit code input with auto-focus and auto-submit
- TOTP validation with ±1 time step window
- Backup codes modal after successful verification (shown once)
- Error handling for invalid/expired codes

**Dependencies**

- PP-1544 (TOTP generation, QR generation, backup code generation)
- PP-1546 (Entry point from MFA selection)
- PP-1554 (Entry point from Account Settings - can be stubbed initially)
- OMS API 4.1 User Authentication with requestNewTOTPkey=true header

**Clarifications Needed**

- Exact route path? (`/mfa/totp/enroll`?)
- Issuer name for otpauth:// URI? ("Proofed Admin" or "Proofed"?)
- Account label format? (email, username, or "Admin Portal"?)
- Backup code count? (8, 10, or configurable?)

**Affected Code Areas**

- `apps/creative-portal/components/pages/mfa-totp-enrollment/` - Enrollment page
- `apps/creative-portal/pages/mfa/totp/enroll.tsx` - Route handler
- `apps/creative-portal/api/mfa/totp/generateSecret.ts` - API endpoint (calls PP-1544 service)
- `apps/creative-portal/api/mfa/totp/verifyEnrollment.ts` - API endpoint (calls PP-1544 service)
- `packages/shared/components/molecules/BackupCodesModal/` - Shared backup codes modal
- `packages/shared/components/atoms/TOTPCodeInput/` - 6-digit input component

**Implementation Steps**

1. Create enrollment page with three-step layout
2. Add API endpoint to generate TOTP secret (calls OMS API with requestNewTOTPkey=true)
3. Add API endpoint to generate QR code (calls PP-1544 QR service)
4. Create 6-digit code input component with auto-submit
5. Add API endpoint to verify enrollment code (calls PP-1544 TOTP validation)
6. On successful verification: generate backup codes, show modal, update MFA state to TOTP_AUTHENTICATOR_APP
7. Create backup codes modal component (copy codes, dismiss)
8. Add error handling for invalid codes
9. Handle navigation away (no state change if not verified)

**Testing Checklist**

- Valid password → new secret returned → QR renders
- Code from app validates within ±1 step; outside window rejected
- Backup codes shown once; not re-fetchable in plaintext
- Manual secret key can be entered in authenticator app
- Navigation away before verification → no MFA state change
- Error messages display correctly for invalid codes

**Estimate**

- Effort: M (2.0 story points)
- Risk: Medium (security-critical enrollment flow)

---

## 5. PP-1548 — TOTP: Enrollment (Customer Portal)

**Why This Ticket Is Implemented At This Stage**

- Enrollment flow for customers. Can be implemented in parallel with PP-1547 since they share logic but are separate portals.

**Summary**

- Implement TOTP enrollment screen with QR code, manual secret entry, code verification, and backup codes modal for Customer Portal.

**Requirements**

- Same as PP-1547 but for Customer Portal
- Entry points: From PP-1545 MFA selection or from Account Settings "Reset authenticator app"
- Three-step enrollment flow
- Backup codes modal
- Uses Client Portal API 4.1 Organization Member Authentication with requestNewTOTPkey=true

**Dependencies**

- PP-1544 (TOTP/QR/backup services)
- PP-1545 (Entry point from MFA selection)
- PP-1553 (Entry point from Account Settings - can be stubbed initially)
- Client Portal API 4.1 Organization Member Authentication

**Clarifications Needed**

- Issuer name for Customer Portal? ("Proofed Customer" or "Proofed"?)
- Can share components with PP-1547?

**Affected Code Areas**

- `apps/customer-portal/components/pages/mfa-totp-enrollment/` - Enrollment page
- `apps/customer-portal/pages/mfa/totp/enroll.tsx` - Route handler
- `apps/customer-portal/api/mfa/totp/generateSecret.ts` - API endpoint
- `apps/customer-portal/api/mfa/totp/verifyEnrollment.ts` - API endpoint
- Shared components from PP-1547 (BackupCodesModal, TOTPCodeInput)

**Implementation Steps**

1. Create enrollment page (reuse shared components from PP-1547 where possible)
2. Add API endpoints (calls Client Portal API instead of OMS API)
3. Same implementation steps as PP-1547 but for Customer Portal context

**Testing Checklist**

- Same as PP-1547 but for Customer Portal
- Correct API integration with Client Portal API 4.1

**Estimate**

- Effort: M (1.0 story points - can reuse components from PP-1547)
- Risk: Medium

---

## 6. PP-1551 — TOTP: Auth and Backup Code Login Flow (Admin & Creative Portal)

**Why This Ticket Is Implemented At This Stage**

- Login flow for users. Depends on PP-1544's TOTP validation and backup code services. Must be implemented before PP-1550 routing.

**Summary**

- Implement TOTP code entry screen and backup code entry screen for Admin/Creative Portal login flow.

**Requirements**

- TOTP login screen: 6-digit numeric input, auto-focus, auto-submit, "Use a backup code" link
- Backup code login screen: backup code input, validation, mark as used
- Error handling for invalid/expired codes
- Lockout support (too many attempts)
- Navigation between TOTP and backup code screens

**Dependencies**

- PP-1544 (TOTP validation, backup code validation)
- PP-1550 (Routing to this flow - can be stubbed initially)
- Existing SMS MFA screen (reuse component structure)

**Clarifications Needed**

- Exact route paths? (`/mfa/totp/verify`, `/mfa/backup-code/verify`?)
- Lockout threshold? (same as SMS: 3 attempts?)
- Should TOTP screen reuse existing SMS MFA component structure?

**Affected Code Areas**

- `apps/creative-portal/components/pages/mfa-totp-verify/` - TOTP verification page
- `apps/creative-portal/components/pages/mfa-backup-code-verify/` - Backup code verification page
- `apps/creative-portal/pages/mfa/totp/verify.tsx` - Route handler
- `apps/creative-portal/pages/mfa/backup-code/verify.tsx` - Route handler
- `apps/creative-portal/api/mfa/totp/verify.ts` - API endpoint (calls PP-1544)
- `apps/creative-portal/api/mfa/backup-code/verify.ts` - API endpoint (calls PP-1544)
- `packages/shared/components/atoms/TOTPCodeInput/` - Reuse from PP-1547

**Implementation Steps**

1. Create TOTP verification page (reuse TOTPCodeInput component)
2. Add API endpoint to verify TOTP code (calls PP-1544 TOTP validation)
3. Create backup code verification page
4. Add API endpoint to verify backup code (calls PP-1544 backup code validation)
5. Implement navigation between screens ("Use a backup code" link)
6. Add error handling and lockout logic
7. On successful verification: set mfaVerified=true, redirect to dashboard

**Testing Checklist**

- Valid TOTP code logs in; invalid/expired code shows error
- Valid backup code works once only; subsequent use fails
- "Use a backup code" link routes correctly
- Lockout triggers after too many attempts
- Error messages display correctly

**Estimate**

- Effort: M (2.0 story points)
- Risk: Medium (security-critical login flow)

---

## 7. PP-1552 — TOTP: Auth and Backup Code Login Flow (Customer Portal)

**Why This Ticket Is Implemented At This Stage**

- Login flow for customers. Can be implemented in parallel with PP-1551 since they share logic.

**Summary**

- Implement TOTP code entry screen and backup code entry screen for Customer Portal login flow.

**Requirements**

- Same as PP-1551 but for Customer Portal
- TOTP and backup code verification screens
- Uses Customer Portal API endpoints

**Dependencies**

- PP-1544 (TOTP/backup validation)
- PP-1549 (Routing to this flow - can be stubbed initially)
- Can share components with PP-1551

**Affected Code Areas**

- `apps/customer-portal/components/pages/mfa-totp-verify/` - TOTP verification page
- `apps/customer-portal/components/pages/mfa-backup-code-verify/` - Backup code verification page
- `apps/customer-portal/pages/mfa/totp/verify.tsx` - Route handler
- `apps/customer-portal/pages/mfa/backup-code/verify.tsx` - Route handler
- `apps/customer-portal/api/mfa/totp/verify.ts` - API endpoint
- `apps/customer-portal/api/mfa/backup-code/verify.ts` - API endpoint

**Implementation Steps**

1. Same as PP-1551 but for Customer Portal context
2. Reuse shared components where possible

**Testing Checklist**

- Same as PP-1551 but for Customer Portal

**Estimate**

- Effort: M (1.0 story points - can reuse components from PP-1551)
- Risk: Medium

---

## 8. PP-1549 — TOTP MFA Challenge Routing After Login (Customer Portal)

**Why This Ticket Is Implemented At This Stage**

- Post-login routing logic. Depends on PP-1544's MFA state management and PP-1552 login flows.

**Summary**

- Implement routing logic to direct customers to correct MFA challenge (SMS, TOTP, or none) after primary authentication.

**Requirements**

- Route based on MFA state:
- No challenge if MFA Override = TRUE or Remember Browser = TRUE
- MFA selection screen if DISABLED or RESETTING
- SMS challenge if MFA set to SMS
- TOTP challenge if MFA set to TOTP
- Only one MFA method ever set per user
- Remember browser respected if MFA is SET

**Dependencies**

- PP-1544 (MFA state helpers: getMFAState, getCurrentMFAMethod)
- PP-1552 (TOTP login flow)
- PP-1545 (MFA selection screen)
- Existing SMS challenge flow
- Existing "Remember Browser" logic

**Clarifications Needed**

- Where does this routing logic live? (middleware, getServerSideProps, or API route?)
- Should this be in login API or separate routing service?

**Affected Code Areas**

- `apps/customer-portal/api/login/index.ts` - Update login logic
- `apps/customer-portal/middleware.ts` - Possibly add routing middleware
- `packages/shared/api/utils/mfa/routing.ts` - Shared routing helpers (if shared with PP-1550)

**Implementation Steps**

1. Create MFA routing helper function (uses PP-1544's getMFAState)
2. Update login API to check MFA state after primary auth
3. Route to appropriate screen based on state:

- TOTP → `/mfa/totp/verify`
- SMS → existing SMS flow
- DISABLED/RESETTING → `/mfa/select`
- Override/Remember → skip MFA, log in

4. Integrate with existing "Remember Browser" logic
5. Ensure only one MFA method is active

**Testing Checklist**

- All MFA states route to correct next screen
- No dual-prompt scenarios
- Lockout respected across flows
- MFA override respected
- Remember browser respected if MFA is SET
- Remember browser not respected if MFA is not SET

**Estimate**

- Effort: S (0.5 story points)
- Risk: Low

---

## 9. PP-1550 — TOTP MFA Challenge Routing After Login (Admin/Creative Portal)

**Why This Ticket Is Implemented At This Stage**

- Post-login routing logic for users. Can be implemented in parallel with PP-1549.

**Summary**

- Implement routing logic to direct users to correct MFA challenge (SMS, TOTP, or none) after primary authentication.

**Requirements**

- Same as PP-1549 but for Admin/Creative Portal
- Route based on MFA state
- Remember browser respected

**Dependencies**

- PP-1544 (MFA state helpers)
- PP-1551 (TOTP login flow)
- PP-1546 (MFA selection screen)
- Existing SMS challenge flow

**Affected Code Areas**

- `apps/creative-portal/api/login/index.ts` - Update login logic
- `apps/creative-portal/middleware.ts` - Possibly add routing middleware
- Shared routing helpers (if shared with PP-1549)

**Implementation Steps**

1. Same as PP-1549 but for Admin/Creative Portal context
2. Reuse shared routing helpers if possible

**Testing Checklist**

- Same as PP-1549 but for Admin/Creative Portal

**Estimate**

- Effort: S (0.5 story points - can reuse logic from PP-1549)
- Risk: Low

---

## 10. PP-1553 — TOTP: Customer Account Settings (Client Portal)

**Why This Ticket Is Implemented At This Stage**

- User-facing MFA management. Depends on enrollment flows (PP-1548) and state management (PP-1544).

**Summary**

- Add "Two-Factor Authentication" settings section in Customer Portal account settings with status display and management actions.

**Requirements**

- Add "Two-Factor Authentication" subsection under "Password" in Account Settings
- Display MFA status: Enabled (TOTP/SMS) or Disabled with icons
- When TOTP active: "Reset authenticator app", "Display backup codes", "Use SMS authentication"
- When SMS active: "Use an App Authenticator"
- Actions route to enrollment flows (PP-1548) or backup codes modal

**Dependencies**

- PP-1544 (MFA state helpers: getCurrentMFAMethod, getBackupCodeCount)
- PP-1548 (Enrollment flow for reset actions)
- PP-1553 mentions "Ticket 4" - need to clarify what this refers to

**Clarifications Needed**

- Exact location in Account Settings? (Which page/route?)
- "Ticket 4" reference - what does this refer to?
- Backup codes display: modal or separate page?

**Affected Code Areas**

- `apps/customer-portal/components/pages/account-settings/` - Account settings page
- `apps/customer-portal/components/molecules/MFASettingsCard/` - MFA settings card component
- `apps/customer-portal/api/mfa/state.ts` - API endpoint to get MFA state
- `apps/customer-portal/api/mfa/backup-codes/get.ts` - API endpoint to get backup codes (if needed)

**Implementation Steps**

1. Add "Two-Factor Authentication" card to Account Settings page
2. Create API endpoint to fetch MFA state and backup code count
3. Display MFA status with appropriate icons (green lock for enabled, red unlocked for disabled)
4. Implement action buttons based on current MFA method:

- TOTP: Reset, Display backup codes, Switch to SMS
- SMS: Switch to TOTP

5. Route actions to enrollment flows or backup codes modal
6. Create backup codes display modal (reuse from PP-1548)

**Testing Checklist**

- Status displayed matches underlying MFA configuration
- Actions route to appropriate flows
- Backup codes display correctly
- UI text uses "authenticator app (TOTP)" consistently
- Reset only resets after successful re-enrollment

**Estimate**

- Effort: M (1.0 story points)
- Risk: Low

---

## 11. PP-1554 — TOTP: User Account Settings (Admin/Creative Portal)

**Why This Ticket Is Implemented At This Stage**

- User-facing MFA management for Admin/Creative Portal. Can be implemented in parallel with PP-1553.

**Summary**

- Add "Two-Factor Authentication" settings card in Admin/Creative Portal account settings with status display and management actions.

**Requirements**

- Same as PP-1553 but for Admin/Creative Portal
- Add "Two-Factor Authentication" card underneath "Password" card
- Same actions and routing as PP-1553

**Dependencies**

- PP-1544 (MFA state helpers)
- PP-1547 (Enrollment flow for reset actions)
- Can share components with PP-1553

**Affected Code Areas**

- `apps/creative-portal/components/pages/account-settings/` - Account settings page
- `apps/creative-portal/components/molecules/MFASettingsCard/` - MFA settings card
- `apps/creative-portal/api/mfa/state.ts` - API endpoint
- Shared components from PP-1553

**Implementation Steps**

1. Same as PP-1553 but for Admin/Creative Portal context
2. Reuse shared components where possible

**Testing Checklist**

- Same as PP-1553 but for Admin/Creative Portal

**Estimate**

- Effort: M (1.0 story points - can reuse components from PP-1553)
- Risk: Low

---

## 12. PP-1555 — TOTP: Admin Customer Management – MFA Status & Reset

**Why This Ticket Is Implemented At This Stage**

- Admin MFA management for customers. Depends on all previous work for complete MFA lifecycle visibility and management.

**Summary**

- Add MFA status display and reset functionality to admin customer management table.

**Requirements**

- Update customer table: Split "MFA Enabled" into detailed status:
- TOTP: "Authenticator"
- SMS: "SMS"
- MFA Override: "Disabled"
- No MFA Set/Resetting: "Not Set"
- Add "Reset MFA" option in ellipses menu (hide if status is "Not Set")
- "Temporarily Override MFA" should work for both TOTP and SMS
- Update ellipses menu order: Unsuspend, Temporarily Disable MFA, Reset MFA, Edit Customer, Deactivate

**Dependencies**

- PP-1544 (MFA state helpers, reset functionality)
- PP-1553 (Customer account settings - for understanding MFA state)
- Existing admin customer management table
- Existing "Temporarily Override MFA" functionality

**Clarifications Needed**

- Where is the admin customer management table? (Which page/component?)
- What API endpoint for resetting MFA? (Admin endpoint or FE server orchestration?)
- Should "Reset MFA" clear TOTP secret, SMS, and backup codes all at once?

**Affected Code Areas**

- `apps/creative-portal/components/organisms/CustomerTable/` - Customer table component
- `apps/creative-portal/components/molecules/CustomerActionsMenu/` - Ellipses menu component
- `apps/creative-portal/api/admin/customers/[id]/mfa/reset.ts` - API endpoint for reset
- `apps/creative-portal/api/admin/customers/[id]/mfa/status.ts` - API endpoint for status (if needed)

**Implementation Steps**

1. Update customer table to display detailed MFA status (replace "MFA Enabled" column)
2. Create API endpoint to get customer MFA status (calls PP-1544 helpers)
3. Add "Reset MFA" option to ellipses menu (conditional display based on status)
4. Create API endpoint to reset MFA (clears TOTP secret, SMS, backup codes, sets state to RESETTING)
5. Update "Temporarily Override MFA" to work with TOTP (currently only SMS?)
6. Reorder ellipses menu items as specified
7. Add vertical dividers for menu formatting

**Testing Checklist**

- Status displayed matches underlying MFA configuration
- "Reset MFA" updates status and forces MFA selection at next customer login
- "Reset MFA" hidden when status is "Not Set"
- No secrets/backup codes exposed in admin UI
- "Temporarily Override MFA" works for both TOTP and SMS
- Menu order matches specification

**Estimate**

- Effort: M (1.0 story points)
- Risk: Low

---

## 13. PP-1556 — TOTP: Admin User Management – MFA Status & Reset

**Why This Ticket Is Implemented At This Stage**

- Admin MFA management for users. Final ticket, depends on all previous work.

**Summary**

- Add MFA status display and reset functionality to admin user management table.

**Requirements**

- Same as PP-1555 but for user (team member) management table
- Update user table MFA status display
- Add "Reset MFA" option in ellipses menu
- Update ellipses menu order: Unsuspend, Reset Password, Temporarily Disable MFA, Reset MFA, View Profile, Edit Team Member, Deactivate

**Dependencies**

- PP-1544 (MFA state helpers, reset functionality)
- PP-1554 (User account settings)
- Existing admin user management table
- Can reuse logic from PP-1555

**Affected Code Areas**

- `apps/creative-portal/components/organisms/UserTable/` - User table component
- `apps/creative-portal/components/molecules/UserActionsMenu/` - Ellipses menu component
- `apps/creative-portal/api/admin/users/[id]/mfa/reset.ts` - API endpoint for reset
- Shared logic from PP-1555

**Implementation Steps**

1. Same as PP-1555 but for user management table
2. Reuse shared logic and components where possible
3. Update menu order as specified (different from customer menu)

**Testing Checklist**

- Same as PP-1555 but for user management
- Menu order matches specification (different from customer menu)

**Estimate**

- Effort: M (1.0 story points - can reuse logic from PP-1555)
- Risk: Low

---

## Parallelization Opportunities

- **PP-1545 and PP-1546** can be implemented in parallel (separate portals, shared logic)
- **PP-1547 and PP-1548** can be implemented in parallel (separate portals, shared components)
- **PP-1549 and PP-1550** can be implemented in parallel (separate portals, shared routing logic)
- **PP-1551 and PP-1552** can be implemented in parallel (separate portals, shared components)
- **PP-1553 and PP-1554** can be implemented in parallel (separate portals, shared components)
- **PP-1555 and PP-1556** can be implemented in parallel (separate entities, shared logic)

## Backend Implementation Status (PP-1344)

**COMPLETED** - Backend support for TOTP is available in:

- OMS Rev 3.16 (TEST environment)
- Client Portal Rev 1.26 (TEST environment)

**Key Backend Details**:

- Secret format: 20-character alphanumeric (A-Z, 0-9) - 160 bits
- Secret encryption: AES (Advanced Encryption Standard)
- Secret storage: OMS database (ProofedUser) and CP database (OrganizationMember)
- Secret retrieval: Only via authentication API response, NOT in lookup APIs
- Header parameter: `requestNewTOTPkey=true` for generation/regeneration

## Blocking Ambiguities

1. **PP-1544**: Backup code character set specification (alphanumeric? special characters?)
2. **PP-1547/PP-1548**: Issuer names ("Proofed" vs "Proofed Admin" vs "Proofed Customer"), account labels (email vs username)
3. **PP-1553**: "Ticket 4" reference needs clarification
4. **PP-1555/PP-1556**: Exact API endpoints for admin MFA operations

## Critical Path

The critical path (must be sequential):

1. PP-1544 (Foundation)
2. PP-1545/PP-1546 (Entry points - can parallelize)
3. PP-1547/PP-1548 (Enrollment - can parallelize)
4. PP-1551/PP-1552 (Login flows - can parallelize)
5. PP-1549/PP-1550 (Routing - can parallelize, depends on login flows)
6. PP-1553/PP-1554 (Account settings - can parallelize)
7. PP-1555/PP-1556 (Admin management - can parallelize)
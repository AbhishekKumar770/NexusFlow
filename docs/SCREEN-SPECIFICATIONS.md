# NexusFlow — Screen Specifications

## 1. Document Purpose

This document defines the purpose, fields, actions, validations, user flows, and security expectations for NexusFlow screens.

The specifications will guide future UI/UX design, database design, API development, and implementation.

**Project Status:** Planning  
**Initial Approach:** Cloud-first, multi-tenant business platform  
**Scope of This Version:** Registration and Login screens

---

## 2. Common Screen Design Principles

All screens should follow these principles:

- Use a consistent layout, typography, spacing, and button style.
- Clearly distinguish required and optional fields.
- Display validation messages close to the relevant fields.
- Show loading indicators during requests.
- Prevent accidental duplicate submissions.
- Provide understandable success and error messages.
- Support responsive layouts for desktop, tablet, and mobile.
- Enforce authentication, authorization, and tenant isolation on the server.
- Never rely on frontend validation alone for security.

---

## 3. Registration Screen

### 3.1 Purpose

Allow a business owner to create a NexusFlow account and initiate registration for a new business tenant.

Registration is self-service. The business owner provides basic business and account information. Additional business configuration takes place after registration.

### 3.2 Screen Information

| Property | Specification |
|---|---|
| Screen Name | Registration |
| Route | `/register` (proposed) |
| Primary User | Business Owner |
| Access | Public |
| Priority | P0 — MVP |
| Next Step | Email Verification |

### 3.3 Fields

| Field | Required | Purpose |
|---|---|---|
| Business / Company Name | Yes | Identify the business |
| Owner Full Name | Yes | Identify the initial account owner |
| Email Address | Yes | Account login and email verification |
| Mobile Number | No | Optional business contact information |
| Password | Yes | Secure account access |
| Confirm Password | Yes | Confirm the entered password |
| Terms and Privacy Consent | Yes | Record acceptance of applicable terms |

Business type, country, timezone, currency, branch, warehouse, and product configuration are excluded from this form. They will be handled in the Business Setup Wizard.

### 3.4 Actions

**Create Account**
- Validate the submitted fields.
- Submit registration to the backend.
- Prevent duplicate processing of the same submission.
- Display the next step only after receiving the appropriate backend response.

**Sign In**
- Navigate to the Login screen.

**Show / Hide Password**
- Allow the user to toggle password visibility without changing its value.

### 3.5 Validation Rules

- Required fields must not be empty.
- Trim unnecessary whitespace from text fields.
- Validate email format on the client and server.
- Check email uniqueness according to the account policy.
- Validate the mobile number when provided.
- Enforce the agreed password policy.
- Confirm Password must match Password.
- Terms and Privacy Consent must be explicitly accepted.
- Enforce reasonable maximum lengths for submitted values.
- Validate all submitted data on the server.

### 3.6 Registration Workflow

1. The business owner opens the Registration screen.
2. The owner enters the required business and account details.
3. The frontend validates basic input.
4. The backend validates the request and checks registration constraints.
5. The backend creates a pending registration without granting normal authenticated access.
6. The system initiates email verification.
7. The user sees a confirmation screen with instructions to verify their email.
8. After successful verification, the account can be activated according to the defined account lifecycle.
9. The user signs in and continues to the Business Setup Wizard.

Tenant and owner account creation must be handled consistently to prevent partially created or duplicate registrations.

### 3.7 Email Verification

- Use a secure, expiring, single-use verification token.
- Do not grant normal authenticated access before verification.
- Allow the user to request a replacement verification email.
- Handle expired, invalid, and already-used verification links.
- Avoid exposing sensitive account information in error messages.
- Apply suitable rate limits to registration and verification requests.

Email delivery is an external operation and must not be represented as successful unless the application has confirmation that the delivery request was accepted by the configured provider.

### 3.8 Expected Error Scenarios

| Scenario | Expected Behaviour |
|---|---|
| Missing required field | Show a field-level validation message |
| Invalid email format | Ask the user to enter a valid email |
| Email already registered | Offer the appropriate sign-in or account recovery path |
| Password mismatch | Ask the user to confirm the password |
| Terms not accepted | Require consent before submission |
| Network or server failure | Show a recoverable error and allow retry |
| Verification email request fails | Explain that the request could not be completed and provide a retry option |
| Duplicate submission | Prevent duplicate tenant or account creation |

---

## 4. Login Screen

### 4.1 Purpose

Allow registered users to authenticate securely and access the NexusFlow tenant they are authorized to use.

### 4.2 Screen Information

| Property | Specification |
|---|---|
| Screen Name | Login |
| Route | `/login` (proposed) |
| Primary User | Registered User |
| Access | Public |
| Priority | P0 — MVP |
| Next Step | Business Setup or Dashboard |

### 4.3 Fields

| Field | Required | Purpose |
|---|---|---|
| Email Address | Yes | Identify the user account |
| Password | Yes | Authenticate the user |
| Remember Me | No | Request an appropriately persistent sign-in session |

### 4.4 Actions

**Sign In**
- Validate required input.
- Submit credentials to the authentication backend.
- Display a loading state during authentication.
- Navigate only after successful authentication and account-status checks.

**Forgot Password**
- Navigate to the password recovery flow.

**Create Account**
- Navigate to the Registration screen.

**Show / Hide Password**
- Toggle password visibility.

**Resend Verification Email**
- Provide an appropriate recovery path when the account has not been verified.

### 4.5 Validation Rules

- Email Address and Password are required.
- Validate the email format.
- Authenticate credentials on the server.
- Do not reveal whether a password or email alone was incorrect.
- Check account verification and activation status.
- Apply suitable rate limits and protections against repeated login attempts.
- Never store plaintext passwords or expose them in logs.

### 4.6 Login Workflow

1. The user opens the Login screen.
2. The user enters their email address and password.
3. The frontend performs basic validation.
4. The backend validates credentials.
5. The backend checks whether the account is verified and active.
6. The application establishes a secure authenticated session or issues appropriate authentication credentials.
7. The application determines the user's authorized tenant context.
8. The user is redirected to the appropriate next screen.

### 4.7 Post-Login Navigation

- If the user's business setup is incomplete, navigate to the Business Setup Wizard.
- If the business setup is complete, navigate to the Dashboard.
- If the account is unverified, provide the email verification flow.
- If the account is disabled or otherwise restricted, deny normal access and show an appropriate message.

The backend must determine access permissions and tenant context. The frontend must not be trusted to select an arbitrary tenant or grant access.

### 4.8 Remember Me

The exact session-persistence strategy will be defined during authentication design.

If enabled, persistence must use secure mechanisms appropriate to the chosen authentication approach. Sensitive credentials must not be stored in browser local storage.

### 4.9 Expected Error Scenarios

| Scenario | Expected Behaviour |
|---|---|
| Missing credentials | Show validation messages |
| Invalid credentials | Show a generic authentication failure message |
| Email not verified | Offer email verification or resend instructions |
| Account disabled | Explain that access is unavailable and provide an appropriate support path |
| Network or server failure | Show a recoverable error |
| Repeated failed attempts | Apply configured security controls and rate limits |

---

## 5. Shared Navigation

- Registration includes a link to Login.
- Login includes a link to Registration.
- Login includes a link to Forgot Password.
- Unverified users can access the verification recovery flow.
- Authenticated users should not be redirected to public authentication screens unnecessarily.
- Post-login navigation depends on account status and business setup status.

---

## 6. Security and Tenant Isolation

- Passwords must be stored using a suitable secure password-hashing algorithm.
- Authentication and authorization must be enforced by the backend.
- Tenant-scoped resources must be isolated by the server.
- Users must not access another tenant's data by changing a URL, request parameter, or identifier.
- Verification and password-recovery tokens must be protected, expire, and be single-use.
- Authentication-related operations should be rate-limited and audited where appropriate.
- Sensitive information must not appear in application logs or error responses.

---

## 7. Dependencies

These screens depend on the following capabilities:

- Tenant creation and lifecycle management.
- User account creation.
- Authentication and authorization.
- Email verification.
- Password recovery.
- Secure session management.
- Tenant isolation.
- Email delivery integration.

The detailed database schema, API contracts, password policy, and session implementation will be documented separately.

---

## 8. Acceptance Criteria

The Registration and Login screens will be considered complete when:

- The agreed fields and navigation are implemented.
- Required-field and format validations work correctly.
- Registration does not create duplicate tenant or owner records.
- Unverified users cannot access protected functionality.
- Verified users can authenticate with valid credentials.
- Invalid credentials do not reveal sensitive account details.
- Post-login navigation respects account and business setup status.
- Tenant isolation is enforced on the server.
- Important success, failure, and recovery paths are tested.
- Relevant documentation is updated.

---

## 9. Out of Scope for This Version

The following are planned for separate specifications:

- Forgot Password screen and complete recovery workflow.
- Email Verification screen details.
- Business Setup Wizard.
- Dashboard and navigation layout.
- User invitation and team onboarding.
- Multi-factor authentication.
- Social login.
- Subscription and billing screens.

---

## 10. Document Status

**Status:** Initial specification drafted for review.

These specifications describe planned behaviour and do not indicate that the screens or backend functionality have been implemented.

---

---

## 11. Forgot Password Screen

### 11.1 Purpose

Allow users to request a secure password reset link when they cannot remember their password.

### 11.2 Screen Information

| Property | Specification |
|---|---|
| Screen Name | Forgot Password |
| Route | `/forgot-password` (proposed) |
| Access | Public |
| Priority | P0 — MVP |

### 11.3 Fields

| Field | Required | Purpose |
|---|---|---|
| Email Address | Yes | Identify the account for password recovery |

### 11.4 Actions

**Send Reset Link**
- Validate the email format.
- Submit the password recovery request.
- Display a generic confirmation message that does not reveal whether the account exists.

**Back to Login**
- Navigate to the Login screen.

**Resend Reset Link**
- Allow another request subject to rate limits and token rules.

### 11.5 Workflow

1. The user opens Forgot Password.
2. The user enters their email address.
3. The backend validates and rate-limits the request.
4. If eligible, the system generates a secure password reset token and initiates email delivery.
5. The screen displays a generic confirmation message.
6. The user opens the reset link received by email.
7. The system validates the token before allowing a password change.

### 11.6 Security Rules

- Never send a password by email.
- Do not reveal whether an email address is registered.
- Reset tokens must be securely generated, single-use, and expire after 30 minutes.
- Apply rate limits to repeated reset requests.
- Do not log reset tokens or expose them in error messages.
- An invalid or expired token must not allow a password change.

---

## 12. Reset Password Screen

### 12.1 Purpose

Allow a user with a valid password reset link to choose a new password.

### 12.2 Screen Information

| Property | Specification |
|---|---|
| Screen Name | Reset Password |
| Route | `/reset-password` (proposed) |
| Access | Public route with a valid reset token |
| Priority | P0 — MVP |

### 12.3 Fields

| Field | Required | Purpose |
|---|---|---|
| New Password | Yes | Set the replacement password |
| Confirm Password | Yes | Confirm the new password |

The reset token will be validated by the backend and will not be treated as a replacement for normal authentication.

### 12.4 Actions

**Reset Password**
- Validate both password fields.
- Verify that the reset token is valid and unused.
- Apply the configured password policy.
- Update the password securely.
- Invalidate the reset token after successful completion.
- Revoke existing login sessions and require the user to sign in again.

**Back to Login**
- Navigate to the Login screen.

**Request New Reset Link**
- Provide a recovery path when the token is expired or invalid.

### 12.5 Workflow

1. The user opens the password reset link.
2. The backend validates the token.
3. The user enters a new password and confirms it.
4. The backend validates the token again when processing the submission.
5. The password is securely updated.
6. The reset token is invalidated.
7. Existing sessions are revoked according to the authentication policy.
8. The user sees a success message and can return to Login.

### 12.6 Validation and Error Handling

- Both password fields are required.
- The password and confirmation must match.
- The new password must satisfy the configured password policy.
- Invalid, expired, or already-used tokens must be rejected.
- Repeated submissions must not reuse a successful reset token.
- Unexpected failures must not expose sensitive information.
- A failed reset must not be reported as successful.

---

## 13. Updated Scope and Document Status

Forgot Password and Reset Password are included in the planned P0 authentication scope.

Email verification, registration, login, and password recovery must work together as a consistent account lifecycle.

**Status:** Specifications drafted for planning; implementation remains pending.


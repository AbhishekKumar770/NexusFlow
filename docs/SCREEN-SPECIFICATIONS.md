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

- Email Verification screen details.
- Dashboard and navigation layout.
- User invitation and team onboarding.
- Multi-factor authentication.
- Social login.
- Subscription and billing screens.

---

## 10. Document Status

Status: Initial screen specifications drafted for planning and review.

This document currently covers Registration, Login, Forgot Password, Reset Password, and the initial Business Setup Wizard. Detailed UI layouts, database schemas, API contracts, and implementation remain separate activities.

These specifications describe planned behaviour and do not indicate that the features have been implemented.

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

---

---

## 14. Business Setup Wizard

### 14.1 Purpose

The Business Setup Wizard helps a newly registered business owner configure the essential settings required to start using NexusFlow.

The wizard covers:

- Business Profile
- Regional Settings
- Branch and Warehouse Setup
- Initial Product and Stock Setup
- Setup Completion

The wizard must remain simple for new businesses while supporting future expansion to multiple branches, multiple warehouses, and additional users.

### 14.2 Screen Information

| Property | Specification |
|---|---|
| Feature | Business Setup Wizard |
| Initial Route | `/setup` (proposed) |
| Primary User | Business Owner |
| Access | Authenticated and authorized users |
| Priority | P0 — MVP |
| Entry Point | After successful registration, email verification, and login |
| Completion Destination | Business Dashboard |

### 14.3 General Behaviour

- The wizard should display clear steps and progress.
- The user should be able to save progress and continue later.
- Previously saved information should be loaded when the user returns.
- The user should be able to navigate to previous steps without losing saved information.
- Required information must be validated before completing the relevant step.
- The user should be able to skip initial product and stock setup.
- The wizard must not create duplicate branches, warehouses, or products when a step is submitted more than once.
- The system should resume the wizard when the user signs in again and setup remains incomplete.
- The system must enforce tenant isolation and permissions on the server.

---

## 15. Business Profile Screen

### 15.1 Purpose

Capture the basic identity and business classification of the registered business.

### 15.2 Screen Information

| Property | Specification |
|---|---|
| Screen Name | Business Profile |
| Parent Feature | Business Setup Wizard |
| Priority | P0 — MVP |

### 15.3 Fields

| Field | Required | Default / Behaviour |
|---|---|---|
| Business / Company Name | Yes | Prefilled from registration; editable |
| Business Type | Yes | User selects a supported type |
| Country | Yes | User selects the business country |
| Industry / Business Category | No | Optional |
| Business Description | No | Not included in the initial version |

### 15.4 Business Type Options

The initial selection list should include:

- Retail
- Wholesale
- E-commerce
- Service
- Other

The available options may expand as NexusFlow supports additional business models.

Business Type and Industry / Business Category must remain separate concepts. For example, a business may be Retail as its business type and Electronics as its industry.

### 15.5 Actions

**Continue**
- Validate required fields.
- Save the business profile.
- Proceed to Regional Settings.

**Back**
- Return to the previous wizard step where applicable.

### 15.6 Validation Rules

- Business Name is required and must not contain only whitespace.
- Business Type is required.
- Country is required.
- Only supported business type and country values may be submitted.
- The backend must validate all submitted values.
- The business owner must only be able to update the business profile belonging to their authorized tenant.

### 15.7 Business Profile Workflow

1. The owner opens the Business Profile step.
2. The registered business name is prefilled.
3. The owner confirms or edits the business name.
4. The owner selects the business type and country.
5. The owner may provide an industry category.
6. The owner saves the information and continues.

### 15.8 Acceptance Criteria

- Registration data is prefilled correctly.
- The owner can edit the business name.
- Required fields are validated.
- The saved profile is available when the owner returns to the wizard.
- The profile is associated with the correct tenant.

---

## 16. Regional Settings Screen

### 16.1 Purpose

Configure the business's base currency, timezone, and preferred date format.

### 16.2 Screen Information

| Property | Specification |
|---|---|
| Screen Name | Regional Settings |
| Parent Feature | Business Setup Wizard |
| Priority | P0 — MVP |

### 16.3 Fields

| Field | Required | Default / Behaviour |
|---|---|---|
| Country | Yes | Prefilled from Business Profile |
| Base Currency | Yes | Suggested using country; owner confirms |
| Timezone | Yes | Suggested using country; owner confirms |
| Date Format | Yes | Suggested default; owner may change |

### 16.4 Default Selection Behaviour

- Country selection should suggest a suitable base currency.
- Country selection should suggest a suitable timezone.
- Country selection should suggest a suitable date format.
- Suggested values must be editable before saving.
- If a country has multiple possible currencies or timezones, the owner must explicitly select the appropriate value.
- Currency and timezone must not be inferred solely from the user's browser or device settings.

Example:

For a business operating in India, the application may suggest INR and Asia/Kolkata. The owner must confirm the selections.

### 16.5 Important Financial Rule

Base currency is a significant financial configuration.

- The selected base currency should be stored explicitly.
- The initial MVP should use one base currency per tenant for core financial reporting.
- Multi-currency transactions and exchange-rate management are future capabilities.
- Changing the base currency after financial transactions exist must require a controlled policy rather than silently converting existing records.

### 16.6 Actions

**Continue**
- Validate and save the regional settings.
- Proceed to Branch and Warehouse Setup.

**Back**
- Return to Business Profile.

### 16.7 Acceptance Criteria

- Suggested values are populated where appropriate.
- The owner can review and change supported settings.
- Invalid currency, timezone, or date-format values are rejected.
- Saved settings are consistently used by the application.
- Date and time values are displayed using the configured preferences.

---

## 17. Branch and Warehouse Setup

### 17.1 Purpose

Create the initial operating location for the business while supporting multiple branches and multiple warehouses within each branch.

### 17.2 Business Structure

The planned relationship is:

Company / Tenant → Branches → Warehouses.

A company may have multiple branches. Each branch may have multiple warehouses.

### 17.3 Initial Branch

The system should create a default branch for a newly configured business.

| Property | Specification |
|---|---|
| Default Branch Name | Head Office |
| Priority | P0 — MVP |
| Editable Name | Yes |
| Additional Branches | Supported as a future extension |

### 17.4 Default Branch Behaviour

- Create one default branch for the tenant during initial setup.
- Use Head Office as the default branch name.
- Allow the owner to rename the branch during setup.
- Ensure the branch belongs to the correct tenant.
- Prevent duplicate default branch creation if the setup step is retried.
- Preserve the branch identity when its name is changed.

Additional branches can be created later through Branch Management.

### 17.5 Default Warehouse

The owner should be able to configure an initial warehouse associated with the default branch.

| Field | Required | Behaviour |
|---|---|---|
| Warehouse Name | Yes | Prefilled with a sensible default, editable |
| Branch | Yes | Associated with the default branch |
| Description | No | Optional |
| Active Status | System-managed | Newly created warehouse is active |

The initial warehouse should be created as part of setup unless the product requirements explicitly allow warehouse creation to be deferred.

### 17.6 Multiple Branches and Warehouses

The future structure must support:

- Multiple branches within one tenant.
- Multiple warehouses within one branch.
- Warehouse creation after the initial setup.
- Additional branches after the initial setup.
- Branch and warehouse activation or deactivation subject to business rules.
- Stock reporting by branch and warehouse.
- Inventory transfers between eligible warehouses.

The initial MVP may implement the default branch and warehouse first. Advanced branch management and transfers can be delivered in later phases.

### 17.7 Important Inventory Rules

- Each warehouse must belong to a valid branch.
- Each branch must belong to a tenant.
- A warehouse must not be associated with a branch belonging to another tenant.
- Stock movements must identify the appropriate warehouse.
- A branch or warehouse containing historical transactions must not be deleted in a way that breaks business records.
- Inactive branches and warehouses must not accept new transactions unless explicitly permitted by policy.

### 17.8 Actions

**Continue**
- Validate and save the branch and warehouse configuration.
- Proceed to Initial Product and Stock Setup.

**Back**
- Return to Regional Settings.

### 17.9 Acceptance Criteria

- The default branch is created exactly once.
- The owner can rename the default branch.
- The initial warehouse is associated with the correct branch.
- Branch and warehouse records are tenant-scoped.
- The data model supports multiple branches and multiple warehouses per branch.
- Repeated submissions do not create duplicate default records.

---

## 18. User Management and User Limits

### 18.1 Purpose

Define how the initial business owner obtains access and how additional employees will be managed.

### 18.2 Initial Owner Account

- The owner account is created during Registration.
- The owner account is associated with the newly registered tenant.
- The owner receives the initial administrative permissions for that tenant.
- The owner must verify their email before normal account activation.
- The owner must not be required to create employee accounts during registration or the initial wizard.

### 18.3 Employee Accounts

Additional users will be managed after registration through User Management.

Planned capabilities include:

- Create or invite users.
- Assign roles and permissions.
- Activate or deactivate user accounts.
- Manage user access to authorized branches and warehouses when supported.
- Review user activity where appropriate.

Advanced invitation workflows and detailed branch-level permission management may be implemented in later phases.

### 18.4 User Count During Registration

The Registration screen must not ask the business owner how many users they need.

The initial MVP will create the owner account only. Additional users can be added later.

### 18.5 Future Subscription-Based User Limits

Subscription-based user limits are planned for a future SaaS phase.

Future capabilities may include:

- Subscription plans.
- Maximum permitted users per plan.
- Current active-user count.
- Usage-limit warnings.
- Upgrade options when limits are reached.

These subscription controls are not part of the initial MVP unless explicitly added to the agreed scope.

### 18.6 Security Rules

- User creation must be authorized.
- Users must belong to the correct tenant.
- A user must not gain permissions simply by changing a request parameter.
- Roles and permissions must be enforced on the server.
- Passwords must never be sent through email as plaintext.
- Employee invitation flows should use secure invitation links rather than emailing passwords.

### 18.7 Acceptance Criteria

- Registration creates the initial owner account.
- The owner can continue setup without creating employees.
- User Management can be introduced independently of Registration.
- Future subscription limits can be added without redesigning the Registration screen.

---

## 19. Initial Product and Stock Setup

### 19.1 Purpose

Allow the business owner to add initial products and optionally record opening stock.

### 19.2 Screen Information

| Property | Specification |
|---|---|
| Screen Name | Initial Product and Stock Setup |
| Parent Feature | Business Setup Wizard |
| Priority | P0 — Basic Setup; advanced import features later |

### 19.3 Available Choices

The owner must be offered two choices:

**Option A — Add Products Now**

The owner may add one or more products and optionally record opening stock.

**Option B — Skip for Now**

The owner may complete the setup wizard without adding products. Products and stock can be added later from their respective management screens.

### 19.4 Initial Product Fields

The initial product form should support:

| Field | Required | Purpose |
|---|---|---|
| Product Name | Yes | Identify the product |
| SKU / Product Code | Based on product policy | Identify the product within the tenant |
| Category | Optional | Classify the product |
| Unit of Measurement | Yes | Define the product's unit |
| Barcode | Optional | Support scanning when available |
| Opening Quantity | Conditional | Record initial stock when supplied |
| Warehouse | Conditional | Identify the warehouse holding opening stock |

Selling price, purchase cost, tax configuration, variants, serial numbers, batch tracking, and advanced pricing require separate product specifications.

### 19.5 Opening Stock Rules

- Opening quantity must be valid for the product's unit.
- Opening stock must be associated with an eligible warehouse.
- Opening stock must create an auditable stock movement or opening-stock transaction.
- The system must not silently change inventory balances without recording the reason and transaction.
- Duplicate product submissions must be handled safely.
- Stock balances must remain consistent with the stock movement history.

### 19.6 Skip Behaviour

When the owner chooses Skip for Now:

- The system must not create dummy products or opening stock.
- The wizard can proceed to completion.
- The owner can create products and record stock later.
- The application should make the product setup entry point discoverable from the dashboard.

### 19.7 Acceptance Criteria

- The owner can add initial products.
- Opening stock is recorded only when valid data is supplied.
- The owner can skip product setup.
- Skipping does not prevent access to the dashboard.
- Product and stock records belong to the correct tenant.
- Stock history remains auditable.

---

## 20. Setup Completion Screen

### 20.1 Purpose

Confirm that the essential business setup is complete and direct the owner to the NexusFlow Dashboard.

### 20.2 Completion Behaviour

After the owner completes the required setup steps:

1. Validate that required business settings exist.
2. Confirm that the default branch exists.
3. Confirm that the initial warehouse exists according to the setup policy.
4. Save the setup completion status.
5. Display a completion message.
6. Provide a Go to Dashboard action.

Product setup may remain incomplete if the owner chose Skip for Now.

### 20.3 Dashboard Navigation

The owner should be able to access the Dashboard after completing required setup.

The dashboard may show onboarding guidance for optional tasks such as:

- Adding products.
- Recording opening stock.
- Creating employee accounts.
- Adding additional branches or warehouses.
- Configuring further business settings.

### 20.4 Incomplete Setup

- If the owner exits before completing setup, saved progress must remain available.
- After the next login, the owner should be redirected to the next incomplete required step.
- The owner must not be forced to repeat completed steps unnecessarily.
- The backend must determine the setup status.

### 20.5 Acceptance Criteria

- The owner can complete the wizard without adding products.
- Required configuration is validated.
- The dashboard is accessible after required setup is complete.
- Incomplete setup can be resumed.
- Repeated completion requests do not create duplicate records.

---

## 21. Business Setup Navigation

The planned flow is:

Registration → Email Verification → Login → Business Profile → Regional Settings → Branch and Warehouse Setup → Initial Product and Stock Setup → Setup Completion → Dashboard.

### Navigation Rules

- Registration creates the pending business registration and owner account.
- Email verification enables normal account activation.
- The authenticated owner starts the Business Setup Wizard.
- Each step saves its data before the user proceeds.
- Back navigation must preserve previously entered data.
- Product setup can be skipped.
- Setup completion must be determined by the backend.
- Users must not be able to mark required setup steps complete merely by changing a frontend value.

---

## 22. Business Setup Data and Integrity Rules

The detailed database design will be documented separately. However, the following business rules must guide that design:

- Every business tenant must have an isolated data scope.
- Every branch must belong to one tenant.
- Every warehouse must belong to a valid branch and tenant.
- The default branch must be created only once per tenant.
- The initial warehouse must be associated with the default branch.
- The owner account must be associated with the correct tenant.
- User permissions must be checked by the backend.
- Product and stock records must belong to the correct tenant.
- Important inventory changes must be recorded in stock history.
- Repeated requests must not create duplicate records.
- Setup status must persist between sessions.
- Financial settings must be stored explicitly and used consistently.

---

## 23. Updated Scope

The initial Business Setup Wizard includes:

- Business Profile.
- Regional Settings.
- Default branch creation and editable branch name.
- Initial warehouse creation.
- Basic initial product creation.
- Optional opening stock entry.
- Skip option for product and stock setup.
- Setup progress persistence.
- Setup completion and dashboard navigation.

The following capabilities are planned for later phases:

- Advanced multi-branch management.
- Advanced warehouse transfers.
- Subscription-based user limits.
- Subscription billing.
- Bulk product imports.
- Advanced product variants and inventory tracking.
- Advanced onboarding automation.
- Self-service plan selection and billing.

---

## 24. Updated Acceptance Criteria

The Business Setup Wizard is ready for implementation when:

- Each screen has an agreed purpose and field list.
- Required and optional fields are identified.
- Navigation and recovery from incomplete setup are defined.
- Default branch and warehouse rules are documented.
- Multi-branch and multi-warehouse relationships are supported by the intended data model.
- User management is separated from initial registration.
- Product setup can be skipped.
- Tenant isolation and authorization requirements are documented.
- Relevant success, failure, and retry paths are specified.
- Database and API designs are completed before the relevant features are implemented.

---

## 25. Business Setup Specification Status

**Status:** Initial specification drafted for planning and review.

These sections describe planned behaviour. They do not indicate that the wizard, branch management, warehouse management, user management, or inventory features have been implemented.



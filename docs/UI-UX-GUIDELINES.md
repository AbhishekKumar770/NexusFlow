# NexusFlow UI/UX Guidelines

## 1. Purpose

This document defines the user interface and user experience standards for NexusFlow.

It establishes consistent design principles for navigation, dashboards, forms, tables, notifications, accessibility, responsive layouts, user preferences, security-related interactions, and onboarding.

These guidelines are the design reference for future frontend implementation. A documented decision does not mean the corresponding feature has already been implemented.

## 2. Core Design Principles

NexusFlow UI/UX must follow these principles:

- Consistency across modules and workflows.
- Clear and predictable navigation.
- Efficient workflows for frequent business operations.
- Readable presentation of business and financial data.
- Responsive behaviour across desktop, tablet, and mobile.
- Accessibility-first design.
- Clear feedback for success, validation, loading, and failure.
- Prevention of accidental data loss and unsafe operations.
- Permission-aware presentation backed by server-side enforcement.
- Localization readiness from the beginning.

## 3. Application Layout

### 3.1 Primary Layout

Use a left-sidebar ERP layout with:

- A top header.
- A left navigation sidebar.
- A central working area.
- Consistent page titles, breadcrumbs where useful, and page-level actions.

The central working area must adapt to the available viewport.

### 3.2 Sidebar Behaviour

- The sidebar is expanded by default.
- Users can collapse it into an icon-only mode.
- Module names must remain discoverable through appropriate tooltips in compact mode.
- The active screen and its parent module must be visually identifiable.
- On mobile, navigation must use a drawer-style interaction.

### 3.3 Module Navigation

Use grouped, expandable modules with relevant submenus.

Illustrative groups include:

- Dashboard
- Master Data
- Inventory
- Purchasing
- Sales
- Purchasing
- Operational Finance
- Reports
- Settings

Dedicated POS navigation and cashier workflows belong to a future phase. Full general-ledger/accounting navigation also belongs to a future phase; MVP finance navigation should expose only approved operational-finance workflows. The exact module and screen hierarchy must remain aligned with the approved requirements and screen specifications.

Only authorized navigation options should be presented. Backend authorization remains authoritative.

## 4. Header

The top header must support:

- Company/Tenant Switcher.
- Branch Selector.
- Notifications access.
- User Profile Menu.
- Global Search.

Tenant and branch selection must be limited to the user's authorized scope. Switching context must not bypass backend authorization or tenant isolation.

## 5. Dashboard

### 5.1 Business Overview

The dashboard should provide a business overview through:

- Summary cards for relevant sales, purchasing, orders, and inventory metrics.
- Sales and purchase trends where sufficient data is available.
- Low-stock alerts.
- Recent transactions.
- Operational-finance summaries, such as receivables, payables, and payment activity, when available and authorized.
- Quick actions relevant to the current user.

Do not imply that formal general-ledger statements (such as trial balance, profit and loss, or balance sheet) are available in the MVP.

Implement summary cards and alerts before introducing more complex visualizations.

### 5.2 Role-Aware Content

Dashboard content and actions should be relevant to the user's role and actual permissions.

Examples include:

- Business Owner: overall business performance.
- Sales Staff: relevant sales activity and customer workflows.
- Inventory Manager: stock status, low-stock alerts, and inventory movements.
- Accountant: authorized financial summaries and payment-related information.

Role alone must never grant access. All data access must be enforced by the backend.

### 5.3 Personalization

Users may show or hide supported dashboard cards and save their preferences.

- Provide role-appropriate default layouts.
- Offer only supported and authorized widgets.
- Keep initial personalization simple.
- Treat drag-and-drop rearrangement as a possible future enhancement.

## 6. Visual Design System

### 6.1 Brand Style

Use a professional blue visual identity with neutral slate and gray surfaces.

- Blue: primary actions, links, active navigation, and important interactive elements.
- Neutral slate/gray: page backgrounds, cards, borders, and tables.
- Green: success states.
- Amber: warnings.
- Red: errors and destructive actions.

Status colours should be restrained and meaningful, not decorative.

### 6.2 Theme

- Light theme is the default.
- Optional dark mode is planned.
- Shared design tokens should support consistent colours, spacing, typography, borders, and components.
- Dark mode implementation timing depends on MVP priorities.
- Both themes must maintain readable contrast and visible focus indicators.

### 6.3 Consistency

Reuse common design tokens and shared components instead of creating unrelated styles in individual modules.

Buttons, form fields, dialogs, tables, alerts, and navigation elements should follow consistent visual rules.

### 6.4 Frontend Component System

The planned frontend uses React, TypeScript, Vite, and Ant Design. Prefer Ant Design components and shared NexusFlow wrappers/design tokens over introducing a second UI component library without an approved reason.

Use React Router for navigation, TanStack Query for server-state fetching and caching, and Ant Design Forms for form state and validation presentation. These tools do not replace backend validation, authorization, or business rules. Use the shared Fetch API client for HTTP calls rather than creating inconsistent request patterns across modules.

## 7. Forms

### 7.1 Layout

Use responsive, section-based forms.

- Desktop: two-column layouts for suitable groups of related fields.
- Mobile: single-column layouts.
- Group related fields into logical sections.
- Use single-column layouts when field relationships or content complexity make them more appropriate.

### 7.2 Transaction Forms

Purchase Orders, Sales Orders, Goods Issue/Delivery Notes, Sales Invoices, and similar transactions should use dedicated line-item tables where appropriate. Keep these as distinct business stages rather than treating an invoice as a substitute for fulfilment.

For the MVP, sales screens should support the approved order, reservation, goods-issue/delivery, invoice, receipt/allocation, and return/refund workflows. Stock should decrease at goods issue/delivery, not merely because an invoice was created. Dedicated POS cashier screens, cash-register sessions, online-store channel screens, real payment-gateway screens, and offline synchronization are future-phase capabilities.

Transaction screens should clearly distinguish:

- Header information.
- Line items.
- Quantities and prices.
- Taxes and discounts, where applicable.
- Totals.
- Payment details, where applicable.
- Final actions.

Financial calculations and transaction validation must be authoritative on the backend.

### 7.3 Validation

- Clearly identify required fields.
- Show validation errors close to the relevant fields.
- Explain how the user can correct an error.
- Preserve valid entered data when possible.
- Make validation feedback accessible to keyboard and assistive-technology users.

### 7.4 Primary Actions

- Place primary form actions at the bottom of the form.
- Keep Cancel or other secondary actions visually subordinate.
- Use sticky action bars for long forms where useful.
- Ensure sticky elements do not cover form content, especially on mobile.

### 7.5 Unsaved Changes

- Detect actual unsaved edits.
- Warn before navigation when unsaved changes would be lost.
- Offer Stay and Discard Changes actions.
- Save explicitly by default.
- Introduce autosave only when a workflow has been specifically designed for it.
- Use supported browser-navigation protections without assuming that every browser transition supports a custom dialog.

## 8. Data Tables and Listings

### 8.1 Standard Table Behaviour

Listings should support, where relevant:

- Server-side pagination.
- Sorting.
- Search.
- Column-specific filters.
- Configurable page size.
- Row-level actions.
- Clear empty, loading, error, and result states.

Search, sorting, filtering, and pagination must work together consistently on the server for large datasets.

### 8.2 Density and Readability

- Comfortable row spacing is the default.
- Maintain readable typography, aligned columns, and sufficient padding.
- Make row actions easy to identify.
- Consider compact mode only as a future enhancement if user needs justify it.

### 8.3 Responsive Tables

Adapt tables to smaller screens using appropriate techniques, such as:

- Horizontal scrolling.
- Prioritized columns.
- Compact layouts.
- Card-style representations where they improve usability.

Do not force every desktop table into an unreadable mobile layout.

## 9. Search

### 9.1 Global Search

Provide global search in the header for supported record types, such as products, customers, invoices, and purchase orders.

### 9.2 Module Search

Each listing must retain appropriate module-specific search, filters, sorting, and pagination.

### 9.3 Search Security

- Search results must respect tenant and branch boundaries.
- Only authorized records and fields may be returned.
- Large datasets should use server-side search.
- Global search should be introduced progressively as supported record types become available.

## 10. Notifications and Feedback

Use context-aware feedback rather than a single message pattern for every operation.

### 10.1 Success

Use temporary toast notifications for successful actions, such as saving a product or updating a purchase order.

### 10.2 Validation

Show field-level validation messages near the relevant controls.

### 10.3 Critical Errors

Use persistent alerts or dialogs when the user must understand a serious failure or take further action.

### 10.4 Notification Centre

Provide a notification centre for important business and system notifications, such as relevant low-stock alerts.

Avoid filling the notification centre with routine success messages.

### 10.5 Reliability

Show success feedback only after the operation has actually succeeded.

Stock and financial operations must use backend validation and appropriate transaction safeguards.

## 11. Loading States

Use loading feedback appropriate to the operation.

- Tables and cards: skeleton placeholders.
- Save and Update actions: button-level progress states.
- Long-running reports and exports: clear status messages.
- Measurable operations: progress indicators when reliable progress information exists.

Do not display invented progress percentages.

When an operation fails, provide useful error feedback and a retry option when retrying is safe.

## 12. Error Handling and Recovery

Errors must be understandable and actionable.

- Explain the issue in user-friendly language.
- Suggest a safe next step.
- Offer retry only when appropriate.
- Provide a support/reference ID for unexpected failures.
- Avoid exposing stack traces, database details, secrets, or sensitive internal information.
- Treat uncertain outcomes of financial or inventory operations carefully before allowing retries.

Backend errors must be mapped to safe, meaningful user-facing messages.

## 13. Destructive Actions

Use risk-based safeguards for destructive and high-impact operations.

- Confirm destructive actions.
- Check dependencies before allowing deletion.
- Prefer deactivate or archive when appropriate.
- Apply stronger confirmation for high-impact operations where justified.
- Protect posted financial transactions and inventory integrity.
- Enforce permissions and dependency rules on the backend.
- Maintain an audit trail for important changes and destructive operations.

A confirmation dialog alone is not sufficient to protect business data.

## 14. Audit History

Provide contextual business change history on relevant records for authorized users.

Where applicable, history should identify:

- Who performed the change.
- When it occurred.
- What changed.

Detailed security and administrative audit logs must be restricted to designated roles.

Audit records must not be editable or deletable by ordinary users. Access and retention policies must be enforced by the backend.

## 15. Data Exports

Support controlled, consistent exports where appropriate.

- CSV/Excel: data analysis and spreadsheet workflows.
- PDF: structured reports suitable for fixed-format presentation.

Large exports may be processed through background jobs; show a clear status and provide the result only when the job has completed successfully. Export availability and formats should follow the approved MVP scope.
- Respect the filters and date ranges applied to the requested data.
- Enforce the user's data and field permissions.
- Preserve tenant isolation.
- Provide status feedback for large exports.
- Avoid exposing unauthorized records or fields.

Not every listing requires every export format. Export options should match the purpose of the screen.

## 16. Accessibility

Use WCAG 2.2 AA as the accessibility design and testing target.

Include:

- Keyboard navigation.
- Visible focus indicators.
- Semantic labels for controls.
- Sufficient colour contrast.
- Accessible form validation.
- Appropriate focus management and keyboard behaviour for dialogs.
- Accessible feedback for loading and error states.

Accessibility must be verified through implementation and testing. Documenting a target does not establish conformance.

## 17. Keyboard Shortcuts

Support standard keyboard accessibility throughout the application.

Introduce a limited set of application-specific shortcuts for frequent tasks, such as:

- Opening global search.
- Saving supported forms safely.
- Selected common workflows.

Document available shortcuts in an accessible reference.

Avoid unnecessary conflicts with browser and operating-system shortcuts. Do not trigger destructive actions through accidental shortcuts.

Finalize exact key combinations during implementation and compatibility testing.

## 18. User Preferences

Allow users to save supported personal working preferences, such as:

- Theme preference, when dark mode is available.
- Default table page size.
- Supported display settings.

Preferences should persist across sessions when implemented.

Personal preferences must never override business rules, tenant boundaries, role permissions, or authorization policies.

## 19. Localization and Regional Settings

Design the interface to support localization from the beginning.

- English is the initial language.
- Keep UI text in localization resources rather than hard-coding labels throughout the application.
- Support regional date and number formatting.
- Display currency using the appropriate currency configuration.
- Respect relevant business timezone settings.
- Plan right-to-left layout support for Arabic.

Presentation formatting must not silently modify stored financial values or transaction currency.

Language, regional display preferences, business timezone, and transaction currency are separate concerns and must be handled accordingly.

## 20. Responsive and Mobile Experience

Provide task-oriented layouts for desktop, tablet, and mobile.

### Desktop

- Full sidebar navigation.
- Detailed tables.
- Multi-section forms.
- Efficient use of the available workspace.

### Tablet

- Adaptive navigation.
- Readable tables.
- Convenient touch controls.

### Mobile

- Drawer navigation.
- Single-column forms.
- Accessible touch controls.
- Task-focused views.
- Appropriately adapted complex tables.

Keep business rules, permissions, and transaction behaviour consistent across devices.

Offline POS and synchronization remain future-phase capabilities and are not implied by responsive mobile support.

## 21. Session Management UX

Session handling must balance security and usability.

- Warn users before expiry where feasible.
- Provide clear messaging when a session expires.
- Require secure re-authentication when necessary.
- Protect against duplicate submissions after expiry or retry.
- Handle unsaved form data safely without bypassing security.
- Avoid indefinitely preserving sensitive data in the browser for recovery.

Session validity and authorization must be enforced by the backend rather than relying only on frontend timers.

## 22. Empty States and Contextual Help

Distinguish between different empty states.

### No Data Yet

Explain that no records exist yet and provide an appropriate next action, such as Add Your First Product.

### No Search Results

Explain that no matching records were found and offer a way to adjust or clear filters.

### Contextual Help

Provide concise screen-level or field-level explanations where they help users complete a task.

Do not show create actions to users who lack the corresponding permission.

## 23. Help and Documentation

Provide both contextual assistance and central documentation.

- Relevant screens may include concise contextual guidance.
- Complex fields may include short explanations.
- Central documentation should support searchable guides, FAQs, and workflow instructions.
- Avoid cluttering the interface with unnecessary tutorials.
- Keep instructions relevant to supported features and authorized workflows.

Documentation must not imply that planned but unimplemented features are available.

## 24. First-Time Onboarding

After the Business Setup Wizard, provide optional guided onboarding.

The experience should include:

- A welcome message confirming setup completion.
- Recommended next steps, such as adding products, configuring inventory, and creating the first transaction.
- The ability to skip guidance.
- The ability to revisit guidance later.
- Recommendations relevant to the user's role, permissions, and setup state.

Users must not be forced through a tutorial after completing the required setup.

## 25. Dashboard and Notification Security

All dashboard cards, global search results, exports, notifications, contextual history, and quick actions must respect authorization and tenant isolation.

Hiding a UI element is not a substitute for backend access control.

Business data must be retrieved only within the authorized tenant, branch, and user scope.

## 26. Implementation Principles

These guidelines define intended UX behaviour; implementation should follow the approved project scope and roadmap.

- Reuse shared UI components and design tokens.
- Implement responsive behaviour consistently.
- Use server-side validation and authorization.
- Keep transaction operations safe against duplicate submissions.
- Provide clear loading, success, and error states.
- Test accessibility and keyboard interaction.
- Validate tenant and branch boundaries.
- Do not claim a feature is complete until it has been implemented and tested.

## 27. Scope and Status

**Current status: Planning and design specification.**

This document records the agreed UI/UX direction for NexusFlow. It does not mean the corresponding interface or functionality has already been developed.

Implementation will proceed in phases according to the approved requirements, screen specifications, architecture, and MVP priorities.

The planned frontend stack is React + TypeScript + Vite + Ant Design, with React Router, TanStack Query, Ant Design Forms, a shared Fetch API client, and i18next/react-i18next for English and Arabic with RTL support. These are implementation decisions, not evidence that the UI has already been built.

The initial MVP is online-first and includes the core ERP interface, inventory, purchasing, sales order/fulfilment/invoicing, and operational-finance workflows. Operational finance includes receivables/payables, receipts and payments with allocations, advances, refunds/returns, cash/bank operations, financial-period lifecycle controls, and operational reports.

Dedicated POS, offline POS and synchronization, e-commerce storefront/channel integration, real payment-gateway integration, and full general-ledger accounting (including chart of accounts, double-entry journals, trial balance, profit-and-loss, and balance-sheet statements) are future-phase capabilities unless separately approved.

Future enhancements, including full dark mode, advanced dashboard customization, and offline POS, must be scheduled explicitly rather than assumed to be part of the initial MVP.

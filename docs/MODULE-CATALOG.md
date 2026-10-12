# NexusFlow — Module Catalog

## 1. Document Purpose

This document defines the planned modules, submodules, screens, forms, user roles, priorities, and dependencies of NexusFlow.

It provides a structured overview of the product before detailed UI design, database design, API development, and implementation begin.

**Project Status:** Planning  
**Product Approach:** Online-first, multi-tenant business management platform  
**Initial Architecture:** Modular Monolith  
**Future Architecture:** Evaluate distributed services when justified by business and technical requirements

---

## 2. Module Priority

| Priority | Meaning |
|---|---|
| P0 — MVP | Essential for the first usable version |
| P1 — Next Phase | Important business capabilities after the initial MVP |
| P2 — Future | Advanced capabilities and future product expansion |

Priorities describe the intended delivery order, not implementation status.

---

## 3. Platform and SaaS Management

**Purpose:** Manage business tenants, company configuration, users, roles, and access to the platform.

### Planned Screens and Forms

| Screen / Form | Purpose | Priority |
|---|---|---|
| Tenant Registration | Register a business account | P0 |
| Tenant Details | Maintain business profile and settings | P0 |
| Login | Authenticate users | P0 |
| Forgot / Reset Password | Support account recovery | P0 |
| User Management | Create and maintain user accounts | P0 |
| Role Management | Define user roles | P0 |
| Permission Management | Control access to modules and actions | P0 |
| Company Settings | Configure company details and preferences | P0 |
| Branch Management | Manage business branches and enforce configured branch-level access | P0 |
| Subscription Management | Manage plans, limits, and billing | P2 |
| Tenant Administration | Support platform-level administration | P2 |

### Primary Roles

- Platform Administrator
- Business Owner / Tenant Administrator
- Business Manager
- Operational User

### Dependencies

Authentication, tenant identification, authorization, and tenant data isolation.

---

## 4. Master Data Management

**Purpose:** Maintain the common business records required by purchasing, inventory, sales, POS, and reporting.

### Planned Screens and Forms

| Screen / Form | Purpose | Priority |
|---|---|---|
| Product List | Search and view products | P0 |
| Product Create / Edit | Maintain product details and SKU | P0 |
| Category Management | Organize products into categories | P0 |
| Unit of Measurement | Define product units | P0 |
| Barcode Management | Assign and maintain product barcodes | P1 |
| Customer List | Search and manage customers | P0 |
| Customer Create / Edit | Maintain customer information | P0 |
| Supplier List | Search and manage suppliers | P0 |
| Supplier Create / Edit | Maintain supplier information | P0 |
| Warehouse Management | Configure stock locations | P0 |
| Tax Configuration | Define tax settings required by MVP transactions | P0 |
| Payment Method Setup | Configure payment methods used by MVP operational finance | P0 |

### Primary Roles

Business Owner, Manager, Inventory User, Purchasing User, Sales User.

### Dependencies

Tenant setup, authorization, and company configuration.

---

## 5. Inventory Management

**Purpose:** Track stock quantities, movements, availability, and adjustments across warehouses and sales channels.

### Planned Screens and Forms

| Screen / Form | Purpose | Priority |
|---|---|---|
| Stock Overview | View current stock balances | P0 |
| Stock by Product | Inspect product-level stock | P0 |
| Stock Movement History | Review stock receipts, issues, and transfers | P0 |
| Goods Receipt | Record received goods | P0 |
| Stock Adjustment | Correct stock differences with reasons | P0 |
| Stock Transfer | Move stock between authorized warehouses | P0 |
| Low Stock Report | Identify products below configured levels | P1 |
| Stock Reservation View | Inspect quantities reserved for orders | P0 |
| Stock Reconciliation | Compare physical and recorded stock with controlled adjustments | P0 |
| Inventory Valuation | Review stock value using the approved moving weighted-average costing policy | P0 |

### Primary Roles

Inventory User, Warehouse Operator, Manager, Business Owner.

### Dependencies

Product master, warehouse setup, purchasing, sales orders, and stock movement rules.

### Important Rules

- Stock changes must be recorded consistently.
- Concurrent transactions must not incorrectly oversell available stock.
- Stock adjustments require appropriate authorization and an audit trail.
- Stock reservation and physical stock movement are separate concepts.

---

## 6. Purchasing and Supplier Management

**Purpose:** Manage supplier purchasing, goods receipt, purchase records, and supplier returns.

### Planned Screens and Forms

| Screen / Form | Purpose | Priority |
|---|---|---|
| Supplier Management | Maintain supplier information | P0 |
| Purchase Order List | Search and review purchase orders | P0 |
| Purchase Order Create / Edit | Create and maintain purchase orders | P0 |
| Purchase Order Details | Review ordered products and quantities | P0 |
| Purchase Approval | Approve orders when configured approval rules apply | P0 |
| Goods Receipt | Record quantities physically received | P0 |
| Supplier Invoice | Record supplier invoice details and match against purchase order and goods receipt | P0 |
| Purchase Return | Record controlled goods returns to a supplier | P0 |
| Supplier Payment | Record supplier payments and allocations | P0 |
| Purchase History | Review previous purchases | P1 |

### Primary Roles

Purchasing User, Warehouse Operator, Finance User, Manager, Business Owner.

### Dependencies

Supplier master, product master, warehouse setup, inventory, and later finance.

### Main Workflow

Create Purchase Order → Receive Goods → Update Stock → Record Supplier Invoice → Record Payable → Record Payment.

The MVP includes purchase orders, approval where applicable, partial goods receipts, supplier invoices, purchase returns, supplier payables, and supplier payment workflows. Apply approved three-way-match and cost-difference rules; do not defer supplier payments and returns.

---

## 7. Sales and Customer Orders (MVP)

**Purpose:** Support customer orders, stock reservation and fulfillment, sales invoicing, operational payments, returns, and refunds. Dedicated POS checkout is a future increment.

### Planned Screens and Forms

| Screen / Form | Purpose | Priority |
|---|---|---|
| Sales Order List | Search and review sales orders | P0 |
| Sales Order Details | View products, quantities, and totals | P0 |
| Create Sales Order | Create a customer order | P0 |
| Order Status | Track order processing | P0 |
| Checkout | Confirm products, pricing, and totals | P1 |
| POS Checkout | Dedicated in-store POS checkout | P2 |
| Sales Invoice | Generate and view sales invoices | P0 |
| Sales Payment | Record customer receipts and allocate payments against eligible balances | P0 |
| Cashier Session | Dedicated POS cashier opening/closing workflow | P2 |
| Sales Return | Record controlled sales returns and linked credit notes | P0 |
| Refund Management | Record controlled refunds against eligible transactions | P0 |
| Discount Management | Apply authorized discounts with server-side validation | P0 |
| Sales History | Review completed transactions | P1 |

### Primary Roles

Sales User, Cashier, Manager, Finance User, Business Owner.

### Dependencies

Product/customer masters, inventory availability, pricing and tax rules, payment methods, authorization, and operational finance. Full general-ledger accounting is a future phase.

### Important Rules

- Recalculate prices and totals on the server.
- Validate stock before confirming transactions.
- Prevent duplicate processing of the same checkout request.
- Keep sales, payment, and stock records consistent.

---

## 8. E-commerce and Order Fulfilment

**Purpose:** Connect online storefronts and online orders with the central business platform.

### Planned Screens and Forms

| Screen / Form | Purpose | Priority |
|---|---|---|
| Online Order List | Review orders received from online channels | P1 |
| Online Order Details | Inspect customer, products, and delivery details | P1 |
| Channel Configuration | Configure supported storefront integrations | P2 |
| Order Import / Sync Status | Review imported orders and synchronization failures | P2 |
| Order Fulfilment | Prepare orders for dispatch | P1 |
| Shipment Management | Track dispatch and delivery information | P2 |
| Online Cancellation | Process eligible order cancellations | P1 |
| Online Return | Manage eligible online returns | P2 |

### Primary Roles

E-commerce User, Sales User, Warehouse Operator, Manager.

### Dependencies

Product catalog, inventory, sales orders, customer data, and integration infrastructure.

### Important Rules

- Online and in-store sales must use consistent stock availability rules.
- Repeated order imports must not create duplicate orders.
- Integration failures must be visible and recoverable.

---

## 9. Operational Finance (MVP) and Full Accounting (Future Phase)

**Purpose:** Provide reliable operational balances, receipts/payments, cash and bank activity, controlled corrections, and operational period-end reporting. Full general-ledger accounting is not part of the MVP.

### Planned Screens and Forms

| Screen / Form | Purpose | Priority |
|---|---|---|
| Chart of Accounts | Define general-ledger accounts | P2 |
| Journal Entry | Record balanced general-ledger entries | P2 |
| General Ledger | Review general-ledger transactions | P2 |
| Accounts Receivable | Track customer receivables and outstanding balances | P0 |
| Accounts Payable | Track supplier payables and outstanding balances | P0 |
| Customer Receipt | Record customer receipts and allocate them against eligible balances | P0 |
| Supplier Payment | Record supplier payments | P1 |
| Expense Entry | Record business expenses | P1 |
| Cash and Bank | Manage cash/bank accounts, transactions, transfers, and opening balances | P0 |
| Manual Reconciliation | Manually reconcile eligible cash/bank transactions with controlled audit history | P0 |
| Trial Balance | General-ledger trial balance | P2 |
| Profit and Loss | Formal accounting statement based on general-ledger postings | P2 |
| Balance Sheet | Formal accounting statement based on general-ledger postings | P2 |
| Tax Reports | Produce applicable operational tax reports; statutory reporting needs separate requirements | P1 |

### Primary Roles

Finance User, Accountant, Manager, Business Owner.

### Dependencies

Company/branch settings, sales, purchasing, payments, returns, cash/bank transactions, financial-period controls, and authorization. General-ledger screens depend on a future chart of accounts and posting rules.

### Important Rules

- Operational financial transactions require controlled correction/reversal and an audit trail.
- Payment allocations and account balances must be validated server-side.
- Financial-period creation, extension, closing, and reopening must follow approved permissions and lifecycle rules.
- Operational reports must not be presented as formal P&L, balance sheet, or general-ledger reports.
- Tax treatment must be configurable for the applicable business jurisdiction.
- General-ledger entries must balance when that future phase is implemented.

---

## 10. Reporting, Audit, and Administration

**Purpose:** Provide visibility into operations, business performance, and important system activities.

### Planned Screens and Forms

| Screen / Report | Purpose | Priority |
|---|---|---|
| Business Dashboard | Summarize key business indicators | P1 |
| Inventory Reports | Review stock and stock movements | P0 |
| Purchase Reports | Review purchasing activity | P1 |
| Sales Reports | Review sales performance | P1 |
| Order Reports | Review order status and fulfilment | P0 |
| Operational Finance Reports | Review receivables, payables, payments, cash/bank activity, and period-end operational balances | P0 |
| Audit Log | Review important user and system actions | P0 |
| Background Job Status | Review asynchronous task execution and job history | P0 |
| Integration Monitoring | Inspect integration errors and retries when integrations are enabled | P1 |

### Primary Roles

Business Owner, Manager, Finance User, Auditor, Platform Administrator.

### Dependencies

Relevant business modules, authorization, and audit/event records.

---

## 11. Notifications and Background Processing

**Purpose:** Handle work that can be processed asynchronously without unnecessarily delaying the user's request.

### Planned Capabilities

| Capability | Purpose | Priority |
|---|---|---|
| Background Job Foundation | Run approved asynchronous work with safe retries and job history | P0 |
| Notification History | Track generated in-app notifications | P0 |
| Email Notifications | Send configured business emails | P1 |
| WhatsApp Integration | Support eligible messaging integrations | P2 |
| Retry Handling | Retry eligible failed operations safely and idempotently | P0 |
| Outbox Pattern | Reliably publish integration events when a concrete workflow requires it | P2 |

### Primary Roles

System Administrator, Business Owner, relevant operational users.

### Dependencies

Core business transactions, background processing, integration configuration, and audit logging.

External messaging and event delivery must not be treated as successful until the relevant provider or processing system confirms success.

---

## 12. Offline-Capable POS — Future Phase

**Purpose:** Allow authorized store users to continue selected POS operations during temporary internet outages.

**Priority:** P2 — Future.

### Planned Screens and Capabilities

| Screen / Capability | Purpose |
|---|---|
| Offline Status Indicator | Show connection and synchronization status |
| Local Product Cache | Make approved product data available offline |
| Offline Sales Queue | Store eligible sales locally |
| Sync Status | Show pending, successful, and failed synchronization |
| Conflict Review | Resolve transactions that require intervention |
| Recovery / Retry | Resume synchronization after interruptions |

### Important Considerations

- Local data persistence and device security.
- Duplicate prevention and idempotent synchronization.
- Conflicts involving stock, pricing, and transaction status.
- Recovery after device or network failures.
- Clear indication of transactions awaiting server confirmation.

Offline operation is a future capability and is not included in the initial MVP.

---

## 13. Future Product Extensions

These modules may be considered after the core platform is stable and validated.

| Module | Potential Purpose | Priority |
|---|---|---|
| Subscription Billing | Manage SaaS plans and recurring billing | P2 |
| Advanced Analytics | Provide deeper business insights | P2 |
| HR and Payroll | Manage employee and payroll operations | P2 |
| Service / Workshop | Manage service jobs and workshop workflows | P2 |
| Contract Management | Track contracts, coverage, and renewals | P2 |
| Manufacturing | Support production and material consumption | P2 |
| Delivery Management | Manage delivery operations | P2 |
| Advanced Multi-Branch Operations | Support more complex branch workflows | P2 |

These are possible extensions, not committed MVP features.

---

## 14. User Roles and Access Principles

The initial role model will include:

- **Platform Administrator:** Platform-level administration.
- **Business Owner / Tenant Administrator:** Business configuration and access management.
- **Manager:** Authorized operational oversight and approvals.
- **Inventory User / Warehouse Operator:** Stock-related operations.
- **Purchasing User:** Purchase-related operations.
- **Sales User / Cashier:** Sales and POS operations.
- **Finance User / Accountant:** Financial transactions and reports.
- **Auditor:** Read-only access to authorized audit and reporting data.

Actual permissions must be configurable and enforced on the server. Hiding a screen or button is not sufficient authorization.

Tenant users must not access another tenant's data. Platform-level access must be explicitly controlled and audited.

---

## 15. Cross-Module Dependencies

The major dependency flow is:

1. Platform foundation: tenant identification, authentication, authorization, company/branch settings, and tenant isolation.
2. Master data: products, units, customers, suppliers, warehouses, tax settings, and payment methods.
3. Inventory foundation: stock balances, movements, transfers, reservations, adjustments, and approved costing rules.
4. Purchasing: orders, approvals where applicable, partial receipts, supplier invoices, returns, payables, and supplier payments.
5. Sales orders and fulfillment: reservations, goods issuance, invoices, returns, credit notes, receipts, and refunds.
6. Operational finance: allocations, advances, cash/bank accounts and movements, manual reconciliation, and financial-period lifecycle.
7. Reporting and reliability: audit, exports, job history, notifications, retries, and operational reports.
8. Future extensions: dedicated POS, full general ledger, e-commerce integrations, offline POS, SaaS billing, and additional modules.

Dependencies will be refined during detailed workflow, screen, data-model, and API design.

---

## 16. Module Completion Criteria

A module should not be considered complete merely because its screen exists.

Depending on the module, completion requires:

- Agreed business workflow and screen specifications.
- Server-side validation and authorization.
- Tenant isolation where applicable.
- Defined database transaction and data-integrity rules.
- Clear success and error handling.
- Audit records for important business actions.
- Appropriate automated tests.
- Updated documentation.
- Review of performance and operational risks.

---

## 17. Scope Management

This catalog describes the planned product structure. It does not mean all listed modules or screens will be implemented immediately.

The initial focus is a reliable online-first MVP. Dedicated POS, full general-ledger accounting, e-commerce integrations, real payment gateways, and offline synchronization are future increments. Detailed field definitions, page layouts, validation messages, API contracts, and database entities will be documented separately.

Architecture decisions, module boundaries, and priorities may change when supported by business requirements and technical evidence.

# NexusFlow — Module Catalog

## 1. Document Purpose

This document defines the planned modules, submodules, screens, forms, user roles, priorities, and dependencies of NexusFlow.

It provides a structured overview of the product before detailed UI design, database design, API development, and implementation begin.

**Project Status:** Planning  
**Product Approach:** Cloud-first, multi-tenant business management platform  
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
| Branch Management | Manage business branches | P1 |
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
| Tax Configuration | Define applicable tax settings | P1 |
| Payment Method Setup | Configure supported payment methods | P1 |

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
| Stock Transfer | Move stock between warehouses | P1 |
| Low Stock Report | Identify products below configured levels | P1 |
| Stock Reservation View | Inspect quantities reserved for orders | P0 |
| Stock Reconciliation | Compare physical and recorded stock | P1 |
| Inventory Valuation | Review stock value using a defined costing policy | P1 |

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
| Purchase Approval | Approve orders when approval rules apply | P1 |
| Goods Receipt | Record quantities physically received | P0 |
| Supplier Invoice | Record supplier invoice details | P1 |
| Purchase Return | Record goods returned to a supplier | P1 |
| Supplier Payment | Record payments to suppliers | P1 |
| Purchase History | Review previous purchases | P1 |

### Primary Roles

Purchasing User, Warehouse Operator, Finance User, Manager, Business Owner.

### Dependencies

Supplier master, product master, warehouse setup, inventory, and later finance.

### Main Workflow

Create Purchase Order → Receive Goods → Update Stock → Record Supplier Invoice → Record Payable → Record Payment.

The first MVP may implement only the basic purchasing and goods-receipt workflow. Invoice accounting and supplier payments can follow in later phases.

---

## 7. Sales and Cloud POS

**Purpose:** Support customer orders, sales transactions, billing, and in-store checkout through a cloud-connected POS.

### Planned Screens and Forms

| Screen / Form | Purpose | Priority |
|---|---|---|
| Sales Order List | Search and review sales orders | P0 |
| Sales Order Details | View products, quantities, and totals | P0 |
| Create Sales Order | Create a customer order | P0 |
| Order Status | Track order processing | P0 |
| Checkout | Confirm products, pricing, and totals | P1 |
| POS Checkout | Process in-store sales | P1 |
| Sales Invoice | Generate and view sales invoices | P1 |
| Sales Payment | Record payments against sales | P1 |
| Cashier Session | Track opening and closing cash balances | P1 |
| Sales Return | Record returned goods | P1 |
| Refund Management | Record refunds against eligible sales | P1 |
| Discount Management | Apply authorized discounts | P1 |
| Sales History | Review completed transactions | P1 |

### Primary Roles

Sales User, Cashier, Manager, Finance User, Business Owner.

### Dependencies

Product master, customer master, inventory availability, pricing, payment methods, and later accounting.

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

## 9. Accounting and Finance

**Purpose:** Provide reliable financial records and reports connected to purchasing, sales, payments, and returns.

### Planned Screens and Forms

| Screen / Form | Purpose | Priority |
|---|---|---|
| Chart of Accounts | Define financial accounts | P1 |
| Journal Entry | Record balanced accounting entries | P1 |
| General Ledger | Review account transactions | P1 |
| Accounts Receivable | Track amounts owed by customers | P1 |
| Accounts Payable | Track amounts owed to suppliers | P1 |
| Customer Receipt | Record customer collections | P1 |
| Supplier Payment | Record supplier payments | P1 |
| Expense Entry | Record business expenses | P1 |
| Cash and Bank | Track cash and bank accounts | P1 |
| Bank Reconciliation | Reconcile recorded and bank transactions | P2 |
| Trial Balance | Review account balances | P1 |
| Profit and Loss | Review income and expenses | P1 |
| Balance Sheet | Review financial position | P1 |
| Tax Reports | Produce reports according to configured tax rules | P2 |

### Primary Roles

Finance User, Accountant, Manager, Business Owner.

### Dependencies

Company financial settings, chart of accounts, sales, purchasing, payments, returns, and accounting posting rules.

### Important Rules

- Every posted journal entry must balance.
- Posted financial transactions must have a controlled correction or reversal process.
- Financial reports must use consistent accounting rules.
- Tax treatment must be configurable for the applicable business jurisdiction.

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
| Financial Reports | Review accounting results | P1 |
| Audit Log | Review important user and system actions | P0 |
| Background Job Status | Review asynchronous task execution | P2 |
| Integration Monitoring | Inspect integration errors and retries | P2 |

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
| Background Job Foundation | Run non-blocking background work | P1 |
| Notification History | Track generated notifications | P1 |
| Email Notifications | Send configured business emails | P2 |
| WhatsApp Integration | Support eligible messaging integrations | P2 |
| Retry Handling | Retry eligible failed operations safely | P1 |
| Outbox Pattern | Reliably publish events after committed database transactions | P2 |

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

1. Platform foundation: tenant identification, authentication, authorization, and company settings.
2. Master data: products, units, customers, suppliers, and warehouses.
3. Inventory foundation: stock balances, stock movements, and reservations.
4. Purchasing and goods receipt: supplier orders and incoming stock.
5. Sales orders: order creation, stock validation, and reservation/release.
6. Cloud POS and payment records: in-store transactions and payment handling.
7. Accounting: financial posting for eligible sales, purchases, payments, and returns.
8. E-commerce integrations: online order ingestion and fulfilment.
9. Reporting and operational reliability: audit, monitoring, retries, and business reports.
10. Future extensions: SaaS billing, offline POS, and additional business modules.

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

The initial focus is a small, reliable, cloud-based MVP. Detailed field definitions, page layouts, validation messages, API contracts, and database entities will be documented separately before their respective implementation phases.

Architecture decisions, module boundaries, and priorities may change when supported by business requirements and technical evidence.

# NexusFlow — Business Workflows

## 1. Purpose

This document describes the planned business workflows for NexusFlow, an online-first, multi-tenant SaaS platform.

The initial MVP requires connectivity to the cloud application. Dedicated POS workflows, e-commerce integrations, real payment-provider integrations, and offline-capable POS synchronization are future increments.

These workflows define intended behavior, not completed functionality.

## 2. Core Workflow Principles

- Each business operates within its own tenant boundary.
- Authorized users can access only the data permitted for their tenant and role.
- Inventory changes must be traceable.
- Orders and payments must follow explicit state transitions.
- Duplicate requests must not create duplicate business operations.
- Business failures must not leave inventory, orders, or financial records inconsistent.
- Financial records must be auditable.
- Asynchronous processing must support retries and duplicate-message handling.
- Offline synchronization must be designed explicitly before offline billing is introduced.

## 3. Tenant Onboarding Workflow

### Flow

1. Platform administrator registers a tenant.
2. The platform assigns a unique tenant identifier.
3. The tenant is activated.
4. A tenant administrator is provisioned.
5. The administrator configures company settings and business users.
6. The administrator assigns roles and permissions.
7. The tenant configures warehouses, products, and other required master data.
8. Authorized users can begin business operations.

### Business Rules

- Tenant identifiers must be unique.
- Inactive tenants cannot perform protected business operations.
- Tenant administrators cannot manage another tenant's users or settings.
- Tenant context must be validated on the server.
- Background jobs and reports must enforce tenant boundaries.

## 4. Product and Inventory Setup Workflow

### Flow

1. An authorized user creates a product.
2. The product is assigned a category and SKU.
3. The tenant configures units, prices, and other required attributes.
4. An authorized user creates a warehouse.
5. Inventory is introduced through an approved opening-stock or stock-receipt workflow.
6. Stock movements and inventory balances become available for authorized queries.

### Business Rules

- Product and warehouse records belong to a tenant.
- SKU uniqueness follows the defined tenant-level business rule.
- Stock changes must have a documented reason and source.
- Opening stock must not silently bypass inventory audit rules.
- Available stock and reserved stock must be tracked separately.

## 5. Purchase Workflow

### Flow

1. An authorized user creates a purchase order.
2. The purchase order records the supplier, items, quantities, prices, and applicable terms.
3. The purchase order is submitted or approved according to the tenant's configured policy.
4. Goods are received against the purchase order.
5. The system records the actual received quantities.
6. Accepted quantities update inventory through stock movements.
7. Purchase invoice information is recorded and matched to the receipt when applicable.
8. The supplier payable is recognized according to the configured accounting policy.
9. Supplier payments and purchase returns are processed through their respective workflows.

### Business Rules

- Ordered, received, invoiced, and returned quantities must be distinguishable.
- Partial receipts must be supported where permitted.
- Duplicate receipt processing must not increase stock twice.
- Purchase cancellation must respect the current receipt and invoice state.
- Inventory and financial postings must be auditable.
- Supplier balances must follow the configured accounting rules.

## 6. Sales Order and Fulfilment Workflow — MVP

### Flow

1. An authorized user creates a sales order for the correct tenant and applicable branch/warehouse.
2. The system validates products, quantities, prices, discounts, and applicable taxes server-side.
3. The order is confirmed or approved according to permissions and configured business rules.
4. Eligible stock is reserved when the order reaches the configured reservation point.
5. Warehouse staff issue goods against the eligible order, including partial fulfillment where permitted.
6. Goods issuance decreases on-hand stock through an auditable stock movement.
7. A sales invoice is created according to the approved invoice-timing rules; invoicing does not independently reduce stock.
8. Customer receipts are recorded and allocated against eligible balances through the operational-finance workflow.
9. The transaction becomes available in operational reports and audit history.

### Business Rules

- The MVP requires connectivity to the cloud application; dedicated cashier/POS checkout is a future increment.
- Prices, discounts, taxes, and payment details must be validated server-side.
- Orders, goods issues, invoices, receipts, and allocations must each be protected against duplicate processing.
- Stock reservations and goods issues must be protected against concurrent operations.
- On-hand stock decreases at goods issue, not merely when an invoice is created.
- Receipts, invoices, and reports must reflect committed transactions.

## 7. Online E-commerce Order Workflow — Future Phase

### Flow

1. A customer places an order through an online storefront or authorized integration.
2. NexusFlow validates the tenant, products, prices, quantities, and order request.
3. The system checks inventory availability.
4. Inventory is reserved according to the order policy.
5. A payment attempt is initiated or recorded.
6. On payment success, the order moves to the appropriate confirmed state.
7. On payment failure, the order follows the defined retry, cancellation, or reservation-release policy.
8. Confirmed orders proceed to picking, packing, and dispatch.
9. Shipment and delivery status are updated through authorized operations or integrations.
10. The order history, inventory movements, and applicable accounting records are maintained.

### Business Rules

- Repeated order requests with the same idempotency key must not create duplicate orders.
- Stock reservation must be concurrency-safe.
- Payment callbacks must be authenticated and processed idempotently.
- Payment status and order status must remain distinct but consistent.
- An order must not be marked as paid solely because a client claims payment succeeded.
- The workflow must define how delayed payment responses and cancellations are handled.

## 8. Shared Inventory Across Sales Channels

### Flow

1. A tenant configures products and warehouse inventory.
2. A POS sale or online order requests stock.
3. The inventory rules validate available quantities.
4. The system reserves or deducts stock through controlled operations.
5. Other sales channels observe the updated availability.
6. Transfers, adjustments, receipts, and returns update stock through their own recorded movements.

### Business Rules

- All channels use consistent inventory rules.
- Concurrent sales must not oversell available stock.
- Stock is scoped to the correct tenant, product, and warehouse.
- Reservations must have explicit lifecycle rules.
- Returns must be inspected or approved according to policy before returned stock becomes sellable.
- Inventory balances must be reconcilable against stock movement history.

## 9. Payment Failure and Order Cancellation Workflow

### Flow

1. An order is created and its stock reservation is recorded.
2. Payment fails, expires, or is cancelled.
3. The system validates whether the order can be cancelled or retried.
4. The order transitions to the appropriate state.
5. The reservation is released when business rules permit.
6. Relevant inventory and order history is recorded.
7. If payment was already captured, the applicable refund workflow is required rather than treating the payment as unpaid.
8. Financial records and payment records are reconciled.

### Business Rules

- Repeated cancellation requests must be safe.
- Stock must not be released twice.
- A captured payment must not be treated as a successful refund without confirmation.
- Refunds must be traceable and linked to the original payment.
- Failed compensation or refund operations must be visible for recovery.

## 10. Sales Returns and Purchase Returns

### Sales Return

1. An authorized user selects the original sale or invoice.
2. The system validates returnable quantities and eligibility.
3. Returned items are inspected where required.
4. Accepted items update inventory according to their condition.
5. A refund, credit, or other permitted settlement is recorded.
6. Applicable operational balance adjustments and audit records are created; full general-ledger postings are a future phase.

### Purchase Return

1. An authorized user selects the original receipt or purchase transaction.
2. Returnable quantities and supplier rules are validated.
3. The returned quantities are recorded.
4. Inventory is reduced through a stock movement where applicable.
5. Supplier credits or payable adjustments are recorded through the operational-finance workflow.
6. The transaction is linked to its original purchase records.

### Business Rules

- Cumulative returns must not exceed eligible original quantities.
- Inventory and financial effects must be linked to the return.
- Repeated requests must not duplicate returns or refunds.
- The system must preserve the original transaction history.

## 11. Operational Finance Workflow (MVP) and General Ledger (Future Phase)

### Operational Finance — MVP

1. Record a customer receipt or supplier payment against the correct tenant and counterparty.
2. Validate the cash/bank account, payment amount, and permitted allocation limits.
3. Allocate receipts/payments against eligible receivable or payable balances; retain unallocated advances where permitted.
4. Record cash/bank transactions, internal transfers, opening balances, and eligible refunds.
5. Perform manual reconciliation against eligible recorded transactions.
6. Apply controlled corrections or reversals and retain audit history.
7. Manage financial periods through the approved create, extend, close, and reopen workflow.
8. Generate operational finance reports from source records.

### Full General Ledger — Future Phase

1. Define the tenant's chart of accounts and posting policies.
2. Determine the accounting entries for qualifying source transactions.
3. Validate and post balanced journal entries linked to source documents.
4. Generate formal trial balance, profit-and-loss, balance-sheet, and general-ledger reports.

### Business Rules

- Operational finance transactions must not be silently overwritten.
- Corrections must use controlled reversal or adjustment workflows.
- Payment allocations and account balances must be validated server-side.
- Financial-period operations must enforce authorization and retain audit history.
- Operational reports must not be represented as formal general-ledger statements.
- When general-ledger functionality is implemented in a future phase, posted journal entries must balance and tax/accounting policies must be configurable for the applicable jurisdiction.

## 12. Future Offline-Capable POS Workflow

Offline billing is a future phase, not part of the initial online-first MVP.

### Planned Flow

1. An authorized POS device downloads the permitted local configuration and required reference data.
2. The device detects loss of connectivity.
3. The cashier records permitted transactions in local storage.
4. Each locally created transaction receives a unique identifier.
5. The POS records the data needed for later synchronization.
6. When connectivity returns, pending transactions are submitted to the cloud.
7. The server validates each transaction for tenant, device, price, stock, and business-rule consistency.
8. Duplicate submissions are detected.
9. Conflicts are handled using explicit policies rather than silently overwriting data.
10. The POS receives synchronization results and records any unresolved exceptions.

### Future Design Requirements

- Secure local persistence
- Device identity and authorization
- Unique transaction identifiers
- Idempotent synchronization
- Conflict detection and resolution
- Price and tax policy for disconnected periods
- Handling of stock sold concurrently at other locations
- Recovery after interrupted synchronization
- Audit history for accepted and rejected transactions

Offline operation must not assume that a locally displayed stock balance remains globally accurate while disconnected.

## 13. Background Jobs and Future Event-Driven Processing

### MVP Background Processing

1. An approved asynchronous task is created with a traceable job identifier.
2. A background worker processes the task using tenant-aware authorization and idempotent handling.
3. Transient failures follow a bounded retry policy.
4. Job status, attempts, outcomes, and persistent failures are recorded for investigation and recovery.
5. Users with permission can review job history and eligible cancellation/recovery options.

### Future Event-Driven Processing

1. When a concrete workflow requires reliable event publication, the source transaction records an outbox message atomically.
2. A background publisher sends pending messages to the selected messaging infrastructure.
3. Consumers validate and process messages idempotently.
4. Operational logs and tracing support diagnosis.

### Business Rules

- Background jobs must be tenant-aware, observable, and safe to retry.
- Duplicate execution must not duplicate business effects.
- When an outbox/broker is introduced, messages may be delivered more than once and consumers must be idempotent.
- Notification failure must not incorrectly reverse an already committed business transaction.
- Future message contracts and versioning must be documented.

## 14. Multi-Tenant Security Workflow

### Flow

1. A user authenticates.
2. The application validates the user, tenant status, and permissions.
3. The server establishes the authorized tenant context.
4. Business operations apply tenant-scoped queries and writes.
5. Background jobs and message consumers validate the tenant context.
6. Unauthorized cross-tenant access is rejected and relevant security events are logged.

### Business Rules

- Client-provided tenant identifiers are not trusted as authorization by themselves.
- Tenant boundaries apply to APIs, reports, exports, background jobs, and asynchronous processing.
- Cross-tenant access scenarios must be covered by automated tests.

## 15. Workflow Status

Status: Initial design.

The online-first MVP workflows—including purchasing, partial receipts, supplier invoices/payables/payments, sales orders, reservations, goods issuance, invoices, operational finance, returns, financial-period controls, audit, exports, and background-job history—will be refined into API contracts, data models, state transitions, and automated acceptance tests before implementation.

Dedicated POS checkout, e-commerce integrations, real payment gateways, full general-ledger accounting, offline POS synchronization, and broker-based distributed workflows will be designed and validated in their respective later phases.

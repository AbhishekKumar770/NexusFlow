## 11. Purchasing and Supplier Management

### US-18: Manage Suppliers
**Priority:** P0

As an authorized tenant user, I want to manage suppliers so that purchases can be associated with the correct supplier.

Acceptance criteria:
- Supplier records belong to a tenant.
- Required supplier information is validated.
- Users cannot access another tenant's suppliers.
- Branch-level access is enforced where configured.
- Important create, update, deactivate, and other permitted changes are auditable.

### US-19: Create a Purchase Order
**Priority:** P0

As authorized purchasing staff, I want to create purchase orders so that inventory replenishment can be planned and tracked.

Acceptance criteria:
- A purchase order contains a supplier and one or more valid items.
- Product quantities and prices are validated server-side.
- Purchase order status follows defined business rules and approval permissions.
- Duplicate requests do not create duplicate purchase orders.
- Changes to approved orders follow controlled rules and leave an audit trail.

### US-20: Receive Purchased Goods
**Priority:** P0

As warehouse staff, I want to record goods received against a purchase order so that accepted quantities update inventory accurately.

Acceptance criteria:
- Receipts identify the relevant purchase order and authorized warehouse.
- Partial receipts are supported according to business rules.
- Accepted quantities update stock through auditable stock movements.
- Duplicate receipt processing cannot increase inventory twice.
- Receipt quantities cannot exceed permitted outstanding quantities without an authorized exception.
- Supplier invoice and payable processing follow the approved operational-finance rules.
- Supplier invoice matching, including purchase-order and goods-receipt matching and cost differences, follows the approved purchasing rules.

### US-33: Process Supplier Invoices and Payables
**Priority:** P0

As authorized purchasing or finance staff, I want to record supplier invoices and maintain supplier payables so that supplier obligations are traceable.

Acceptance criteria:
- An invoice is associated with the appropriate supplier and relevant purchase documents where applicable.
- Duplicate supplier invoices are detected using the configured tenant-level uniqueness rules.
- Matching discrepancies and cost differences are recorded and handled through controlled rules.
- Payable balances are updated consistently and can be traced to source records.
- Tenant and configured branch permissions are enforced.

### US-34: Record Supplier Payments
**Priority:** P0

As authorized finance staff, I want to record supplier payments and allocate them against eligible supplier balances so that outstanding payables remain accurate.

Acceptance criteria:
- A payment is associated with the appropriate tenant, supplier, and cash/bank account.
- Full and partial payments and permitted allocations are supported.
- Amounts, account status, payment date, and allocation limits are validated.
- Duplicate requests do not record a payment or allocation twice.
- Changes and reversals follow controlled, auditable workflows.

### US-35: Process a Purchase Return
**Priority:** P0

As authorized purchasing staff, I want to return eligible purchased goods so that inventory and supplier balances remain accurate.

Acceptance criteria:
- The return references the original receipt or purchase transaction.
- Returned quantities cannot exceed eligible quantities, considering prior returns.
- Stock movements are recorded when applicable and reflect the item's disposition.
- Supplier credits and payable adjustments follow the approved operational-finance rules.
- Duplicate requests cannot process the same return twice.
- The return and any resulting balance changes are auditable.

## 12. Sales Orders and Fulfilment (MVP)

### US-21: Create and Manage a Sales Order
**Priority:** P0

As authorized sales staff, I want to create and manage sales orders so that customer demand can be tracked before goods are issued.

Acceptance criteria:
- An order belongs to the correct tenant and uses an authorized branch and warehouse where applicable.
- Products, quantities, prices, discounts, and applicable taxes are validated server-side.
- The order follows defined status-transition and authorization rules.
- Confirming or approving an order reserves eligible stock according to the approved reservation policy.
- Repeated requests do not create duplicate orders or reservations.
- Cancellation releases eligible reservations through an auditable operation.

### US-22: Issue Goods Against a Sales Order
**Priority:** P0

As authorized warehouse staff, I want to issue goods against an eligible sales order so that fulfilled quantities and inventory are recorded accurately.

Acceptance criteria:
- Only eligible orders and quantities can be issued.
- Partial fulfillment is supported according to business rules.
- Goods issuance decreases on-hand stock through an auditable stock movement.
- Concurrent issues cannot cause invalid stock balances or overselling.
- Duplicate requests cannot issue the same quantities twice.
- The issued quantities and remaining order quantities remain traceable.

### US-23: Create a Sales Invoice
**Priority:** P0

As authorized sales staff, I want to create an invoice for eligible sales so that invoiced amounts can be tracked separately from physical stock movement.

Acceptance criteria:
- The invoice references the appropriate sales order and/or goods issue according to the approved invoice-timing rules.
- Invoice totals, discounts, and applicable taxes are validated server-side.
- Creating an invoice does not independently decrease on-hand stock.
- Duplicate requests do not create duplicate invoices.
- Invoice status and permitted corrections follow defined, auditable rules.

### US-24: Record Customer Receipts and Allocate Payments
**Priority:** P0

As authorized sales or finance staff, I want to record customer receipts and allocate payments against eligible balances so that collections and outstanding receivables remain accurate.

Acceptance criteria:
- A receipt is associated with the appropriate tenant, customer, and cash/bank account.
- Full and partial receipts, permitted allocations, and customer advances are supported.
- Payment amounts, account status, and allocation limits are validated server-side.
- Duplicate requests do not create duplicate receipts or allocations.
- Unallocated amounts remain identifiable.
- Corrections and reversals follow controlled, auditable workflows.

## 13. E-commerce and Online Orders (Future Phase)

### US-25: Receive an Online Order
**Priority:** P2 — Future

As an online sales channel, I want to submit orders to NexusFlow so that online purchases can use the platform's shared order and inventory rules.

Acceptance criteria:
- Incoming requests are authenticated and validated.
- Products and prices are validated server-side.
- Duplicate requests with the same idempotency key do not create duplicate orders.
- Inventory reservation follows the shared inventory rules.
- Order status reflects the defined payment and fulfilment workflow.
- Integration failures are visible and can be safely retried.

### US-26: Manage Order Fulfilment for Online Channels
**Priority:** P2 — Future

As warehouse staff, I want to process confirmed online-channel orders for dispatch so that customer orders can be fulfilled and tracked.

Acceptance criteria:
- Only eligible orders can enter fulfilment.
- Picking and dispatch information is recorded.
- Order status transitions follow defined rules.
- Failed or repeated updates do not create inconsistent fulfilment history.
- Integration and synchronization failures are visible for investigation.

## 14. Operational Finance (MVP) and Full Accounting (Future Phase)

### US-27: Track Receivables and Payables
**Priority:** P0

As authorized finance staff, I want to track customer receivables and supplier payables so that outstanding business obligations can be reviewed.

Acceptance criteria:
- Balances are linked to the appropriate customer, supplier, and source transactions.
- Payments, allocations, advances, returns, and eligible adjustments update balances consistently.
- Reports respect tenant and configured branch boundaries.
- Reconciliation discrepancies can be investigated.
- Changes to posted operational transactions follow controlled reversal or adjustment workflows.

### US-28: Manage Cash and Bank Transactions
**Priority:** P0

As authorized finance staff, I want to manage cash and bank accounts, transactions, transfers, and opening balances so that operational funds are traceable.

Acceptance criteria:
- Cash/bank accounts belong to the correct tenant and follow configured access rules.
- Opening balances, receipts, payments, and internal transfers are validated.
- Transfers record both sides consistently and cannot be duplicated.
- Account balances can be traced to their transactions.
- Corrections and reversals are controlled and auditable.

### US-29: Reconcile Operational Cash and Bank Records
**Priority:** P0

As authorized finance staff, I want to manually reconcile eligible cash/bank transactions so that discrepancies can be identified and investigated.

Acceptance criteria:
- Reconciliation is restricted to authorized tenant data and eligible transactions.
- Reconciliation status and relevant notes are recorded.
- Repeated actions do not duplicate reconciliation effects.
- Changes to completed reconciliation follow controlled rules and retain audit history.

### US-30: Manage Financial Periods
**Priority:** P0

As an authorized business owner or finance user, I want to manage financial periods so that operational transactions follow controlled period boundaries.

Acceptance criteria:
- Monthly periods are the default unless the tenant's approved configuration specifies otherwise.
- A sensible initial date range is created automatically; the system does not pre-create an entire year by default.
- Periods can be extended through a controlled workflow when business dates require it.
- Closing and reopening require appropriate permission and an audit trail.
- Transactions in closed periods follow the approved rejection or controlled-reopen policy.
- Period reports respect tenant boundaries and validated date ranges.

### US-31: Review Operational Finance Reports
**Priority:** P0

As an authorized business owner, I want to review operational finance reports so that I can understand receivables, payables, receipts, payments, cash/bank activity, and period-end operational balances.

Acceptance criteria:
- Reports use documented calculations based on operational source records.
- Reports are restricted to authorized tenant and branch data.
- Date and financial-period filters are validated.
- Totals can be traced to underlying records.
- Operational reports are not represented as formal profit-and-loss statements, balance sheets, trial balances, or general-ledger reports.

### US-36: Maintain a Chart of Accounts (Future Phase)
**Priority:** P2 — Future

As authorized finance staff, I want to maintain a chart of accounts so that future general-ledger transactions can be classified consistently.

Acceptance criteria:
- Accounts have defined codes, names, and account types.
- Account codes follow the tenant's configured uniqueness rules.
- Inactive accounts cannot be used for new postings where prohibited.
- Access is restricted by role and tenant.

### US-37: Post Balanced General-Ledger Journal Entries (Future Phase)
**Priority:** P2 — Future

As finance staff, I want business transactions to produce balanced journal entries so that future general-ledger records remain consistent and auditable.

Acceptance criteria:
- Each posted journal entry has balanced debit and credit totals.
- Journal entries are linked to source transactions where applicable.
- Invalid or unbalanced entries cannot be posted.
- Posted entries cannot be silently overwritten.
- Corrections use controlled reversal or adjustment workflows.

### US-38: Review Formal Accounting Statements (Future Phase)
**Priority:** P2 — Future

As an authorized business owner, I want to review formal accounting statements so that I can understand the business's financial position and performance after general-ledger accounting is implemented.

Acceptance criteria:
- Statements use validated and documented general-ledger calculations.
- Reports are restricted to authorized tenant data.
- Date and reporting-period filters are validated.
- Financial totals can be traced to underlying posted entries.

## 15. Returns, Credit Notes, and Refunds (MVP)

### US-39: Process a Sales Return
**Priority:** P0

As authorized sales staff, I want to process eligible sales returns so that returned goods and customer settlements are recorded correctly.

Acceptance criteria:
- The return references the original sale, invoice, or goods issue as required by the approved workflow.
- Cumulative returned quantities cannot exceed eligible quantities, considering previous returns.
- Stock is updated according to the item's condition and return policy.
- Refunds, credit notes, and operational balance adjustments are traceable to source records.
- Duplicate requests cannot process the same return twice.
- Return, stock, and settlement changes are auditable and use controlled correction/reversal rules.

### US-40: Issue a Credit Note or Refund
**Priority:** P0

As authorized finance or sales staff, I want to issue eligible credit notes and refunds so that customer settlements are corrected consistently.

Acceptance criteria:
- The credit note or refund references the original eligible transaction.
- Amounts cannot exceed the remaining eligible balance without an explicitly authorized exception.
- Refund method and linked cash/bank movement are recorded where applicable.
- Duplicate requests do not create duplicate credits or refunds.
- Corrections and reversals are controlled and auditable.

## 16. Future Dedicated and Offline-Capable POS

### US-41: Process a Sale Through Dedicated POS
**Priority:** P2 — Future

As a cashier, I want to process a sale through a dedicated POS so that physical-store checkout can use shared product, pricing, sales, payment, and inventory rules.

Acceptance criteria:
- The cashier can select products and quantities.
- Prices, discounts, and applicable taxes are validated server-side.
- The sale uses the appropriate tenant, branch, and warehouse.
- Stock changes are protected against concurrent sales.
- A finalized sale cannot be processed twice because of a repeated request.
- A receipt or invoice can be generated from the recorded transaction.

### US-31A: Record Sales While Disconnected
**Priority:** P2 — Future

As a cashier, I want to record permitted sales when internet connectivity is unavailable so that store operations can continue during an outage.

Acceptance criteria:
- Offline transactions are stored securely on an authorized device.
- Each transaction has a unique identifier.
- The device records the information needed for later synchronization.
- The interface clearly indicates that transactions are pending synchronization.
- Offline stock information is not represented as globally current.

### US-32A: Synchronize Offline Transactions
**Priority:** P2 — Future

As a business owner, I want offline transactions to synchronize safely when connectivity returns so that local and cloud records can be reconciled.

Acceptance criteria:
- Repeated submissions do not duplicate transactions.
- Tenant and device authorization are validated.
- Conflicts are detected and handled using documented rules.
- Interrupted synchronization can resume safely.
- Rejected or unresolved transactions remain visible for investigation.

## 17. SaaS and Business Extensions

**Priority:** P2

Future user stories may cover:
- Tenant subscription and billing management
- Advanced multi-branch configuration and reporting
- Customer self-service portal
- Real payment-provider integration
- Email and WhatsApp delivery
- Advanced analytics and reporting
- HR, payroll, contracts, workshop, delivery, and manufacturing modules
- Independently deployed microservices where justified

Each capability requires detailed acceptance criteria before implementation.

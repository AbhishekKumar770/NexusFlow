# NexusFlow — Requirements

## 1. Purpose

NexusFlow is a planned multi-tenant, cloud-based business management platform designed to support online and offline business operations through a unified system.

The long-term vision includes product and inventory management, purchasing, sales, point of sale (POS), e-commerce, accounting, finance, reporting, and optional industry-specific modules. The finalized MVP decisions define the initial release boundary; capabilities described as future extensions are not included in the MVP unless explicitly approved.

The initial release will focus on a secure, reliable, online-first business foundation. Offline-capable POS functionality and synchronization are planned for a later phase.

These requirements describe intended behavior and do not imply that the functionality has already been implemented.

## 2. User Roles

### Platform Administrator
- Manage tenant registrations and platform-level settings.
- Activate or deactivate tenants.
- Review platform health and operational information.
- Manage platform-level configuration and onboarding.

### Tenant Administrator
- Manage users and roles within their own tenant.
- Configure company settings, branches, products, and warehouses.
- Manage tenant-specific business configuration.
- Access permitted business reports and audit information.

### Tenant Staff
- Perform authorized purchasing, inventory, sales, POS, and order operations.
- View permitted business records.
- Process transactions according to assigned permissions.

### Finance Staff
- Manage permitted financial transactions, receivables, payables, reconciliations, and reports.
- Review accounting entries and source documents according to permissions.

### Customer
- Represent a buyer associated with a tenant's business.
- Have customer information accessed only through authorized workflows.
- Customer self-service login is outside the initial MVP unless explicitly added to scope.

## 3. Functional Requirements

### FR-01: Tenant Management
- Each business must belong to a distinct tenant.
- Tenant-specific records must be associated with the correct tenant.
- Tenant users must not access another tenant's private business data.
- Tenant activation status must control access to protected business operations.
- Tenant context must be validated server-side.
- Tenant boundaries must also apply to background jobs, reports, exports, and asynchronous messages.

### FR-02: Authentication and Authorization
- Users must authenticate before accessing protected endpoints.
- Roles and permissions must control access to protected operations.
- Invalid or expired credentials must not authorize access.
- User management operations must respect tenant boundaries.
- Sensitive operations must validate permissions server-side.

### FR-03: Company, Branch, and Warehouse Management
- Authorized users must be able to maintain company and branch information.
- Warehouses must belong to the appropriate tenant.
- Inventory operations must identify the relevant warehouse where applicable.
- Access to branches and warehouses must follow configured permissions.
- Multi-branch functionality will be introduced incrementally.

### FR-04: Product Catalog
- Create, view, update, and deactivate products.
- Assign products to categories.
- Maintain a unique SKU within the defined business scope.
- Support units of measure and product attributes where required.
- Validate required fields and invalid input.
- Preserve relevant product references used by historical transactions.

### FR-05: Inventory Management
- Track stock by tenant, product, and warehouse.
- Distinguish available stock from reserved stock.
- Record stock receipts, issues, adjustments, transfers, and other permitted movements.
- Prevent reservations that exceed available stock.
- Protect inventory updates against concurrent operations.
- Maintain traceable stock movement history.
- Release reservations when permitted order cancellation or payment-failure rules require it.
- Introduce inventory valuation and reconciliation rules before financial production use.

### FR-06: Supplier and Purchasing Management
- Create and maintain supplier records.
- Create purchase orders containing products, quantities, prices, and applicable terms.
- Support controlled purchase approval and cancellation rules.
- Record goods receipts, including partial receipts.
- Update inventory through controlled stock movements; goods receipt is the stock-in event.
- Record purchase invoice information, supplier balances, and due dates.
- Support purchase returns, supplier payments, supplier advances, and controlled payment allocation within the finalized operational-finance scope.
- Support supplier invoice matching against purchase orders and goods receipts, with controlled handling of price or quantity differences.
- Prevent duplicate processing of goods receipts and other critical purchasing requests.

### FR-07: Customer Management
- Create and maintain customer records.
- Associate customers and orders with the appropriate tenant.
- Validate customer information before order creation.
- Support customer balances and transaction history when the relevant modules are implemented.
- Restrict customer data to authorized tenant workflows.

### FR-08: Sales and Order Management
- Create orders containing one or more products.
- Validate product availability, prices, quantities, discounts, and applicable taxes.
- Maintain order status and status history.
- Support order cancellation according to defined business rules.
- Prevent duplicate processing of the same idempotent request.
- Keep payment status distinct from order status.
- Record applicable inventory and financial effects.

### FR-09: Point of Sale (Future Increment)
- Dedicated POS workflows are outside the initial MVP.
- If introduced in a later increment, POS must use shared product, pricing, tax, inventory, payment, and sales rules.
- Authorized cashiers must be able to select products, enter quantities, and calculate transaction totals.
- The future POS must maintain traceable cashier and transaction history.
- Returns and refunds must follow the shared controlled return and refund workflows.
- Offline-capable POS and synchronization remain a separate future phase.

### FR-10: E-commerce and Online Orders
- Accept online orders through an authorized storefront integration or API.
- Validate tenant, products, prices, quantities, and order details.
- Use shared inventory and order-processing rules where appropriate.
- Support payment-provider integration in a later phase.
- Plan for fulfilment, shipping, cancellation, and returns.
- Handle repeated requests and payment callbacks safely.

### FR-11: Operational Payments and Payment Processing
- The MVP supports the finalized operational payment workflows, including supported payment methods, receipts, supplier payments, manual allocations, customer and supplier advances, refunds, and cash/bank transaction links where specified by the MVP decisions.
- Payment records must have explicit statuses and relevant transaction references, and critical requests must be safe against duplicate processing.
- Sales-order reservations must follow the finalized order lifecycle; payment failure must not release stock unless the applicable business rule explicitly requires it.
- A mock payment-provider flow may be used for learning and testing, but it does not replace operational payment recording.
- Real external payment-provider integration is deferred until its security, reconciliation, and operational requirements are defined.

### FR-12: Operational Finance (MVP) and Full Accounting (Future Phase)
- The MVP includes the finalized operational-finance scope: customer receivables and supplier payables, payment recording and allocation, advances, refunds, cash/bank account masters and transactions, internal transfers, opening balances, and manual reconciliation as specified by the MVP decisions.
- Link operational financial transactions to their source business documents and preserve traceable correction/reversal history.
- Protect posted transactions from silent overwrite; use controlled reversals or compensating transactions where required.
- The MVP does not include a full general ledger, chart of accounts, double-entry accounting engine, or formal profit-and-loss and balance-sheet statements.
- Full accounting, jurisdiction-specific accounting compliance, and formal financial statements are future-phase capabilities requiring separate approved requirements.

### FR-13: Returns, Credits, Refunds, and Cancellations
- Validate return eligibility against the original transaction.
- Prevent cumulative returned quantities from exceeding eligible quantities.
- Record accepted and rejected returned quantities where applicable.
- Update stock according to the return condition and policy.
- Record eligible customer credit notes, supplier credits, customer refunds, and supplier refunds within the finalized operational-finance scope.
- Make repeated cancellation, return, credit, and refund requests safe against duplicate processing.
- Formal general-ledger accounting entries for these operations remain outside the MVP.

### FR-14: Notifications and Asynchronous Processing
- Generate relevant business events for eligible transactions.
- Process applicable notifications asynchronously.
- Introduce reliable event publication and consumption where required.
- Retry transient failures using bounded retry policies.
- Record persistent failures for investigation and recovery.
- Prevent notification failure from incorrectly reversing a committed business transaction.

### FR-15: Reporting and Audit
- Provide basic order, purchase, and inventory summaries.
- Introduce sales and financial reports with their respective modules.
- Maintain stock movement and transaction history.
- Record important business actions for auditing.
- Restrict reports and exports to authorized tenant data.
- Ensure reported totals follow documented calculation and accounting rules.

### FR-16: Future Offline-Capable POS
Dedicated POS workflows are outside the initial MVP, and offline POS functionality is not part of the initial online-first MVP.

Before implementation, the system must define:
- Secure local persistence and device authorization.
- Unique identifiers for locally created transactions.
- Synchronization after connectivity is restored.
- Duplicate submission prevention.
- Conflict detection and resolution.
- Handling of stock availability while disconnected.
- Recovery from interrupted synchronization.
- Auditable handling of rejected or unresolved transactions.

Offline stock information must not be assumed to represent globally current inventory while a device is disconnected.

### FR-17: Future SaaS Productization
- Support controlled tenant onboarding and configuration.
- Evaluate subscription plans and billing requirements.
- Provide tenant lifecycle and operational management.
- Define backup, restore, monitoring, and support procedures.
- Evaluate deployment and database-isolation options according to actual client requirements.

### FR-18: Future Business Extensions
Potential future modules include HR, payroll, contract management, service/workshop operations, delivery, and manufacturing.

Each extension requires its own requirements, user stories, workflows, permissions, and acceptance criteria before implementation.

## 4. Non-Functional Requirements

### Security
- Enforce tenant isolation on every relevant data-access path.
- Validate input and apply authorization checks.
- Protect credentials, tokens, API keys, and connection-string secrets.
- Use secure configuration and appropriate secret management.
- Record relevant security-sensitive events.

### Reliability
- Handle duplicate requests safely.
- Prevent inventory overselling under concurrent operations.
- Preserve consistency between orders, inventory, payments, and financial postings.
- Support reliable event publication and processing when asynchronous workflows are introduced.
- Use bounded retries and documented recovery procedures.

### Maintainability
- Organize code into clear business modules.
- Use dependency injection and appropriate separation of concerns.
- Avoid unnecessary abstractions and premature microservices.
- Document significant architecture decisions and known limitations.

### Testing
- Test important domain rules with unit tests.
- Test persistence and API workflows with integration tests.
- Include insufficient-stock, concurrent-reservation, duplicate-request, and unauthorized-tenant scenarios.
- Test payment failures, cancellations, returns, and recovery paths as relevant modules are introduced.
- Add synchronization and conflict tests before offline POS is released.

### Observability
- Use structured logging.
- Provide application health checks.
- Add tracing and messaging diagnostics when asynchronous and distributed components are introduced.
- Avoid logging secrets or unnecessary sensitive customer data.

### Operations and Recovery
- Define backup and restore procedures before production use.
- Validate deployment and database migration procedures.
- Monitor application failures and important background-processing outcomes.
- Document operational recovery steps.

## 5. Initial MVP Scope

The initial MVP will prioritize:

1. Tenant and user foundation.
2. Authentication, authorization, and tenant isolation.
3. Company, branch, warehouse, and product master data.
4. Inventory movements, reservations, transfers, adjustments, and moving weighted-average valuation rules as specified by the MVP decisions.
5. Customer and supplier master data, including opening balances where applicable.
6. Purchasing, purchase approvals, partial goods receipt, supplier invoices, purchase returns, and supplier payment workflows.
7. Sales orders, stock reservations, partial fulfillment, goods issuance, sales invoices, returns, credit notes, and controlled cancellations.
8. Operational payments, allocations, advances, refunds, cash/bank transactions, and manual reconciliation within the approved MVP scope.
9. Financial-period lifecycle controls and operational period-end reports, without a full general ledger.
10. Controlled CSV imports/exports, background jobs, in-app notifications, and audit histories as specified by the MVP decisions.
11. React + TypeScript + Ant Design + Vite frontend, integrated with the ASP.NET Core Web API.
12. API documentation, automated tests, security checks, observability, backups, restore validation, and controlled deployment.

The finalized MVP decisions define the detailed boundary and dependencies. Dedicated POS workflows, full accounting, e-commerce storefront integration, real payment-provider integration, and offline POS synchronization remain future increments unless separately approved.

Dedicated POS workflows, full accounting/general ledger, e-commerce storefront integration, real payment-provider integration, and offline POS synchronization will be introduced in subsequent increments. Operational-finance features explicitly included in the finalized MVP decisions are not deferred by this statement.

## 6. Frontend Technology

- The MVP frontend will use React with TypeScript.
- Vite will be used for frontend development and production builds.
- Ant Design will provide the primary UI component system for forms, tables, navigation, dialogs, and other ERP screens.
- TanStack Query will manage server state and API data fetching/caching.
- React Router will manage client-side navigation and protected route presentation; server-side authorization remains authoritative.
- A shared Fetch API client will centralize API communication and error handling.
- Ant Design Forms will be used for standard form workflows, with business rules also validated by the backend.
- Frontend tests will use Vitest, React Testing Library, and Playwright.
- English and Arabic localization, including right-to-left layout, are part of the planned frontend requirements.
- Frontend architecture must follow the finalized MVP scope; selecting a frontend technology does not imply every future business module is included in the MVP.

## 7. Acceptance Criteria

The initial MVP is functionally ready when:

- Authorized users can perform permitted operations.
- Cross-tenant data access is rejected.
- Products and warehouses can be managed.
- Basic purchasing and goods receipt can update inventory safely.
- Orders can be created and tracked.
- Concurrent inventory reservations cannot oversell available stock.
- Duplicate requests do not create duplicate orders or receipts.
- Failed payment simulation triggers the defined reservation-release behavior.
- Stock movements and important actions are traceable.
- Automated tests cover the principal success and failure workflows.

## 8. Out of Scope for the Initial MVP

- Real payment gateway integration
- Production email or WhatsApp delivery
- Offline-capable POS and synchronization
- Production-scale multi-region deployment
- Kubernetes orchestration
- Immediate decomposition into multiple microservices
- Dedicated POS frontend/workflows (the core React + TypeScript + Ant Design + Vite ERP frontend is part of the MVP)
- Full accounting and tax compliance for every jurisdiction
- All optional industry-specific modules

## 9. Status

Status: Planning.

These requirements describe intended capabilities. No feature should be considered implemented until its acceptance criteria are met and the implementation is tested.

Detailed user stories, API contracts, data relationships, accounting rules, and edge-case behavior will be refined before implementing each module.

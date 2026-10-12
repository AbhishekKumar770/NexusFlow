# NexusFlow — Project Plan

## 1. Project Overview

NexusFlow is a planned multi-tenant, cloud-based business management platform designed for online-first business operations through a unified system. Offline-capable POS and synchronization are future-phase capabilities.

The long-term vision is to manage products, inventory, purchasing, sales, point of sale (POS), e-commerce orders, accounting, finance, reporting, and other business operations within one platform.

NexusFlow will initially be developed as a portfolio and learning project, with the longer-term goal of evaluating it for real business use and potentially offering it to other businesses as a SaaS product. Production use requires completion of the defined security, testing, operational, backup, recovery, and deployment criteria.

The initial implementation will focus on a reliable, online-first core. Offline-capable POS functionality and synchronization will be planned for a later phase.

## 2. Problem Statement

Businesses may use separate systems for physical sales, online orders, inventory, purchasing, and financial records. This can create inconsistent stock balances, duplicate transactions, disconnected reporting, and difficulties maintaining accurate business records.

NexusFlow aims to provide a unified platform with consistent business rules, traceable transactions, controlled access, and reliable integration between operational and financial workflows.

The platform must also protect each tenant's data and support future growth without introducing unnecessary architectural complexity at the beginning.

## 3. Product Goals

- Build REST APIs using C# and ASP.NET Core Web API.
- Use Entity Framework Core and SQL Server for persistence.
- Implement secure authentication, authorization, and multi-tenant data isolation.
- Manage products, warehouses, suppliers, customers, purchases, inventory, and orders.
- Support the approved MVP business workflows, with dedicated POS and e-commerce integrations delivered in later increments.
- Maintain accurate stock balances and prevent overselling.
- Implement the approved operational-finance workflows with auditable transactions; full general-ledger accounting is a future phase.
- Prevent duplicate processing and handle business failures consistently.
- Introduce asynchronous processing and reliable event handling where appropriate.
- Add automated testing, containerized development, CI/CD, logging, and monitoring.
- Begin with a modular monolith and evaluate microservices extraction when justified.
- Keep future offline POS and synchronization requirements in the architecture roadmap.
- Evaluate production readiness before using the platform for real business operations.

## 4. Planned Business Modules

### Platform Foundation
- Tenant management and onboarding
- Authentication and authorization
- User, role, and permission management
- Company, branch, and warehouse configuration
- Audit history and system configuration

### Product and Inventory Management
- Product and category management
- SKU, units of measure, and barcode support
- Warehouse and stock management
- Stock movements, adjustments, and transfers
- Inventory reservations and availability
- Inventory valuation and reconciliation

### Purchasing and Supplier Management
- Supplier records
- Purchase orders and approvals
- Goods receipt
- Purchase invoices and supplier balances
- Purchase returns and supplier payments

### Sales and Customer Management (MVP)
- Customer records
- Sales orders and their lifecycle
- Sales order reservations, partial fulfillment, and goods issuance
- Sales invoices, discounts, and applicable tax rules
- Sales returns, credit notes, refunds, and controlled cancellations

### Point of Sale (Future Increment)
- Dedicated POS billing and cashier workflows
- POS-specific reconciliation and receipt workflows, using shared sales and inventory rules

### E-commerce and Order Fulfilment (Future Increments)
- Online storefront integration
- External online-order ingestion
- Shared inventory availability and order rules
- Real payment-provider integration
- Picking, packing, shipping, and delivery tracking
- Order cancellations and returns through shared business workflows

### Operational Finance (MVP)
- Customer receivables and supplier payables within the approved operational scope
- Payment receipts, supplier payments, manual allocations, and customer/supplier advances
- Cash and bank account masters, transactions, internal transfers, and opening balances
- Controlled refunds, transaction reversals, and manual reconciliation
- Operational period-end reporting and financial-period lifecycle controls

### Full Accounting (Future Phase)
- Chart of accounts
- Double-entry general ledger and balanced journal entries
- Formal profit-and-loss, balance-sheet, and other accounting statements
- Jurisdiction-specific accounting compliance and expanded accounting policies

### Reporting and Integrations
- Sales, purchase, and inventory reports
- Operational dashboards
- Financial reporting
- API integrations and webhooks
- Email and messaging notifications

### Future Business Extensions
- HR and payroll
- Service and workshop management
- Contract management
- Delivery operations
- Manufacturing and other industry-specific functionality

These modules represent the long-term product vision. They are not all part of the initial MVP.

## 5. Initial MVP Scope

The initial MVP will include:

1. Tenant, user, authentication, authorization, and tenant-isolation foundation
2. Company, branch, warehouse, product, customer, and supplier master data
3. Inventory movements, stock adjustments, transfers, reservations, and moving weighted-average valuation rules
4. Purchasing, approvals, partial goods receipts, supplier invoices, purchase returns, and supplier payment workflows
5. Sales orders, reservations, partial fulfillment, goods issuance, sales invoices, returns, credit notes, and controlled cancellations
6. Operational payments, allocations, advances, refunds, cash/bank transactions, and manual reconciliation within the approved MVP scope
7. Financial-period lifecycle controls and operational period-end reports; no full general ledger
8. Controlled CSV imports/exports, background jobs, in-app notifications, and audit histories
9. React + TypeScript + Ant Design + Vite frontend integrated with the ASP.NET Core Web API
10. API documentation, automated tests, tenant/security checks, observability, backups, restore validation, and controlled deployment

Dedicated POS workflows, full accounting/general ledger, e-commerce storefront integration, real payment-provider integration, and offline POS synchronization are future increments unless separately approved. The finalized MVP decisions define the detailed requirements and dependencies.

## 6. Primary Business Workflows

### Purchasing
1. Create a purchase order.
2. Record goods received.
3. Update inventory through controlled stock movements.
4. Record purchase invoice information.
5. Recognize supplier liabilities according to the accounting policy.
6. Process supplier payments and purchase returns.

### Sales Orders and Fulfilment (MVP)
1. Create a sales order and validate customer, products, quantities, prices, discounts, and applicable taxes.
2. Confirm/approve the order according to permissions and reserve available stock.
3. Issue goods, including partial fulfillment where permitted; stock on-hand decreases at goods issue.
4. Create sales invoices according to the configured invoice timing; invoicing does not independently change stock.
5. Record payment receipts and allocate them against eligible balances.
6. Process eligible returns, credit notes, refunds, cancellations, and linked reversals.

### Point of Sale (Future Increment)
Dedicated cashier/POS workflows will use the shared sales, inventory, tax, payment, and return rules.

### E-commerce Orders
1. Receive an order through an authorized integration.
2. Validate tenant, products, prices, and quantities.
3. Reserve available inventory.
4. Process or record payment.
5. Confirm, cancel, or retry according to the defined rules.
6. Process fulfilment, returns, and applicable financial effects.

### Operational Finance (MVP)
1. Record operational receipts, supplier payments, advances, allocations, refunds, and cash/bank movements.
2. Link transactions to their source business documents where applicable.
3. Apply permissions, balance validation, and controlled correction/reversal rules.
4. Perform manual reconciliation and financial-period lifecycle operations.

### Full Accounting (Future Phase)
Full general-ledger posting and formal financial statements require separate approved requirements.

## 7. Technology Plan

- Language: C#
- Backend: ASP.NET Core Web API
- ORM: Entity Framework Core
- Database: SQL Server
- API documentation: OpenAPI/Swagger
- Authentication: ASP.NET Core Identity and an appropriate token-based authentication approach
- Background processing: Hangfire with SQL Server storage for approved asynchronous jobs; introduce a message broker only if a demonstrated workflow requires it
- Testing: xUnit and integration testing
- Containers: Docker
- CI/CD: GitHub Actions
- Observability: Structured logging, health checks, and distributed tracing where appropriate
- Frontend: React + TypeScript, Ant Design, and Vite
- Server-state management: TanStack Query
- Routing: React Router
- API communication: Shared Fetch API client
- Forms: Ant Design Forms
- Frontend testing: Vitest, React Testing Library, and Playwright
- Localization: English and Arabic with RTL support

Technology versions, platform compatibility, and security recommendations will be verified during environment setup and implementation.

## 8. Architecture Strategy

Begin with a modular monolith with explicit business-module boundaries and clear ownership of business rules.

Use appropriate abstractions and automated tests without adding patterns or infrastructure that do not solve a real requirement.

Introduce reliable asynchronous processing, an outbox, idempotent consumers, and bounded retries where business workflows require them.

Evaluate microservices only when independent deployment, scaling, data ownership, and operational readiness justify the additional complexity.

Future offline POS functionality will require explicit design for local persistence, transaction identity, synchronization, conflict resolution, and recovery.

## 9. Non-Functional Requirements

- Secure tenant isolation
- Authentication and role-based authorization
- Concurrency-safe inventory operations
- Idempotent order, payment, and message processing
- Consistent and auditable business transactions
- Reliable event publishing and consumption
- Bounded retries and recoverable failures
- Automated tests for important business rules
- Secure configuration and secret management
- Backup and recovery planning
- Useful logs, health checks, and operational documentation
- Performance and deployment validation before production use

## 10. Development Roadmap

### Phase 1: Requirements and Design
- Align product vision, requirements, user stories, and workflows.
- Define MVP boundaries and dependencies.
- Design core domain entities and relationships.
- Document architecture decisions and security boundaries.

### Phase 2: Development Foundation
- Verify the development environment.
- Create the solution structure.
- Configure SQL Server and EF Core.
- Establish authentication, tenant context, and authorization.
- Add initial automated tests and CI checks.

### Phase 3: Core Business Operations
- Implement catalog and warehouse management.
- Implement stock movements, reservations, and concurrency protection.
- Implement customer and supplier records.
- Implement basic purchasing and goods receipt.
- Implement orders, payment simulation, cancellation, and audit history.

### Phase 4: Operational Finance and Period Controls
- Complete operational payments, allocations, advances, refunds, and cash/bank workflows included in the MVP.
- Implement manual reconciliation and financial-period lifecycle controls.
- Complete operational reports without presenting them as formal accounting statements.

Dedicated POS and full general-ledger accounting remain future phases.

### Phase 5: Dedicated POS and E-commerce Extensions
- Evaluate and introduce dedicated POS workflows after the core sales flows are stable.
- Add storefront integration and online-order ingestion when approved.
- Add real payment-provider integration only after security and reconciliation requirements are defined.
- Validate cancellations, refunds, fulfilment, and failure scenarios.

### Phase 6: Reliability and Integrations
- Establish background processing, job status history, safe retries, cancellation, notifications, and observability as required by the MVP decisions.
- Use idempotent job handling and explicit recovery procedures.
- Introduce an outbox and reliable event publication when a workflow needs atomic database-change/event-publication guarantees.
- Add a message broker only when the architecture and operational need justify it.
- Expand integration and failure-path testing.

### Phase 7: SaaS Productization
- Improve tenant onboarding and configuration.
- Evaluate subscription and plan management.
- Add operational monitoring, backup and recovery procedures.
- Validate security, performance, deployment, and support processes.

### Phase 8: Architecture Evolution
- Evaluate extraction of selected modules into microservices.
- Define independent data ownership where services are extracted.
- Validate distributed failure handling and deployment automation.

### Phase 9: Offline-Capable POS
- Design local persistence and device authorization.
- Implement safe transaction identifiers and synchronization.
- Handle conflicts, duplicate submissions, and interrupted synchronization.
- Validate stock and payment behavior during disconnected operation.

### Phase 10: Future Business Extensions
- Evaluate HR, payroll, contracts, workshop, delivery, manufacturing, and other modules based on actual business needs.

## 11. Definition of Done

A feature is complete only when:
- Its requirements and acceptance criteria are met.
- Business validation and authorization are implemented.
- Relevant success and failure scenarios are tested.
- Important security and tenant-isolation cases are covered.
- API and workflow documentation are updated.
- The implementation is reviewed and its known limitations are recorded.

## 12. Current Status

Status: Planning.

The repository currently contains planning documentation. The business modules and workflows described in this document are planned capabilities, not implemented features.

The MVP scope, data model, architecture decisions, and implementation sequence will be refined before coding begins.

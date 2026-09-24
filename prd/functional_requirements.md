# Functional Requirements

## 1. Tenant & Merchant Mangagement

- **FR-TEN-01** Platform admins can create, edit, suspend, reactivate, and close a tenant, which is merchant.
- **FR-TEN-02** Each tenant has a profile: legal name, display name, default currency, default locale, timezone, and contact details.
- **FR-TEN-03** A suspended tenant's storefront and APIs reject new orders, while existing data stays readable by the tenant's admin.
- **FR-TEN-04** Every resource (user, customer, product, order, etc.) belongs to exactly one tenant, and all reads and writes are scoped to the caller's tenant.

## 2. Merchant Users && Access Control

- **FR-USR-01** A tenant owner can invite, deactivate, and remove staff users.
- **FR-USR-02** Staff users authenticate with email + password, with optional MFA.
- **FR-USR-03** Access is role-based. Built-in roles: Owner, Admin, Inventory Manager, Order Manager, Support, Read-only.
- **FR-USR-04** A tenant can create custom roles from a fixed set of permissions.
- **FR-USR-05** A user can belong to multiple tenants and switch between them; permissions are evaluated per tenant.
- **FR-USR-06** Tenants can issue and revoke API keys with scoped permisions for integration.
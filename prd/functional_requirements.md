# Functional Requirements

## 1. Tenant & Merchant Mangagement

- **FR-TEN-01** Platform admins can create, edit, suspend, reactivate, and close a tenant, which is merchant.
- **FR-TEN-02** Each tenant has a profile: legal name, display name, default currency, default locale, timezone, and contact details.
- **FR-TEN-03** A suspended tenant's storefront and APIs reject new orders, while existing data stays readable by the tenant's admin.
- **FR-TEN-04** Every resource (user, customer, product, order, etc.) belongs to exactly one tenant, and all reads and writes are scoped to the caller's tenant.

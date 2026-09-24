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
- **FR-USR-06** Tenants can issue and revoke API keys with scoped permissions for integration.

## 3. Customer Management

- **FR-CUS-01** Customers can register, sign in, reset their password, and manage their profile.
- **FR-CUS-02** Customers can check out as guests; a guest can later be linked to a registered account by verified email.
> TODO: Need confirmation of the orders on the verified customer side before linking orders??? Anonymous user can create guest orders with a certain email of a certain user.
- **FR-CUS-03** Customers can save multiple shipping and billing addresses and choose a default.
- **FR-CUS-04** Merchant staff can search, view, create, edit, and deactivate customers, and see a customer's order history.
- **FR-CUS-05** Customers identities are per tenant: the same email at two tenants is two separate customers.
- **FR-CUS-06** Customers can request export or deletion of their personal data; deletion anonymizes personal fields on historical orders instead of removing the orders.

## 4. Product Catalog & Variants

- **FR-CAT-01** Merchants can create, edit, publish, unpublish, and archive products.
- **FR-CAT-02** A product has a title, description, media, categories/collections, tags, and custom attributes.
- **FR-CAT-03** A product defines option axes (e.g. size, color); each purchasable combination is a variant with its own SKU, price, weight, and inventory.
- **FR-CAT-04** SKUs are unique within a tenant.
- **FR-CAT-05** Merchants can organize products into hierarchical categories and manual or rule-based collections.
- **FR-CAT-06** Customers can browse, search, and filter published products (by keyword, category, attribute, price, availability).
- **FR-CAT-07** Merchants can bulk import and export products via CSV.
- **FR-CAT-08** Archived products are hidden from the storefront but remain referenced by past orders.

## 5. Inventory

## 6. Pricing

## 7. Cart & Checkout

## 8. Orders

## 9. Payments & Refunds

## 10. Shipping & FulFillment

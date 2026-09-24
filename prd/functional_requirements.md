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
- **FR-CUS-05** Customer identities are per tenant: the same email at two tenants is two separate customers.
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

- **FR-INV-01** Merchants can define one or more stock locations.
- **FR-INV-02** Stock is tracked per variant per location as on-hand, reserved, and available, which is (on-hand - reserved).
- **FR-INV-03** Stock is reserved when an order is placed and released when the order is cancelled or payment fails / times out.
- **FR-INV-04** Stock is deducted from on-hand when items are fulfilled.
- **FR-INV-05** Per variant, the merchant chooses whether to allow overselling (backorders, if so, should be able to set limit) or block checkout when unavailable.
- **FR-INV-06** Merchants can manually adjust stock with a required reason; every change is recorded as an inventory movement.
- **FR-INV-07** Merchants can set a low-stock threshold per variant and receive notifications when it is crossed.

## 6. Pricing

- **FR-PRC-01** Each variant has a base price, and optionally a compare-at price, in the tenant's currency.
- **FR-PRC-02** Merchants can create discounts: percentage off, fixed amount off, free shipping, and buy-X-get-Y rule.
- **FR-PRC-03** Discounts can be automatic or code-based, and can be limited by date range, minimum order value, eligible products/collections, customer, and usage count (total and per customer).
- **FR-PRC-04** The system defines how multiple discounts combine (stacking rules) and applies them deterministically.
- **FR-PRC-05** Taxes are calculated per order line from tax rules configured by shipping destination and product tax category; prices can be configured as tax-inclusive or tax-exclusive.
- **FR-PRC-06** The price, discounts, and taxes applied to an order are snapshotted at checkout and do not change if the catalog or rules change later.

## 7. Cart & Checkout
- **FR-CHK-01** Customers can add, update, and remove items in a cart; carts persist for signed-in customers across sessions.
- **FR-CHK-02** A guest cart is merged into the customer's cart on sign-in.
> TODO: Should determine the confirmation method.
- **FR-CHK-03** At checkout the customer provides contact, shipping address, shipping method, billing address, and payment; the system shows an itemized total (subtotal, discounts, shipping, tax, grand total).
- **FR-CHK-04** Checkout re-validates price, discounts, and availability before placing the order, and tells the customer about any change.

## 8. Orders

## 9. Payments & Refunds

## 10. Shipping & Fulfillment

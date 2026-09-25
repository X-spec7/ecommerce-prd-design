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
- **FR-ORD-01** Placing an order creates an order with a tenant-unique, human-readable order number.
- **FR-ORD-02** An order has separate statuses for payment (pending, authorized, paid, partially refunded, refunded, failed), fulfillment(unfulfilled, partially fulfilled, fulfilled), and overall lifecycle (open, cancelled, closed).
- **FR-ORD-03** Merchant staff can view, search, and filter orders, and add internal notes.
- **FR-ORD-04** Merchant staff can edit an unfulfilled order (add/remove items, change quantities or address), with price, tax, inventory, and payment adjusted accordingly.
> TODO: Should determine whether audit record for this will be required.
- **FR-ORD-05** Orders can be cancelled before fulfillment; cancellation releases reserved stock and voids or refunds payment.
- **FR-ORD-06** Merchant staff can create orders manually on behalf of a customer (draft orders) and send a payment link.
- **FR-ORD-07** Customers can view their order history, order status, and tracking information.
- **FR-ORD-08** Customers can request a return for fulfilled items within a merchant-configured window; merchants approve or reject it, and receiving returned items can restock them.

## 9. Payments & Refunds

- **FR-PAY-01** The system integrates with external payment providers through a provider-agnostic interface; the platform never stores raw card data.
> TODO: Consider building a simple mock stripe service.
- **FR-PAY-02** Each tenant connects its own payment provider account(s)
- **FR-PAY-03** Supported flows: authorize and capture together, or authorize now and capture later (e.g. at fulfillment)
- **FR-PAY-04** Authorizations not captured within the provider's window are handled (captured, re-authorized, or voided) per tenant policy.
- **FR-PAY-05** Merchant staff can issue full or partial refunds optionally per line item and including shipping; total refunds cannot exceed the captured amount.
- **FR-PAY-06** Every payment operation (authorize, capture, void, refund) is recorded as a transaction with provider reference, amount, status, and timestamps.
- **FR-PAY-07** Asynchronous provider notifications (webhooks) are processed idempotently and reconcile transaction and order payment status.
- **FR-PAY-08** Payment failures are shown to the customer with a retry option; unpaid orders expire after a configurable timeout.

## 10. Shipping & Fulfillment

- **FR-SHP-01** Merchant can define shipping zones (by country/region) and shipping rates per zone (flat, weight-based, or order-value-based, including free-shipping thresholds).
- **FR-SHP-02** Checkout shows only shipping methods valid for the destination and cart.
- **FR-SHP-03** Merchant staff can fulfill an order fully or partially, from a chosen stock location, creating a fulfillment (shipment) with the fulfilled line items.
- **FR-SHP-04** A fulfillment records carrier, tracking number, and tracking URL, and can be marked shipped, delivered, or cancelled.
- **FR-SHP-05** Carrier label purchase and live-rate lookup go through a provider-agnostic interface (integration optional in v1).

> TODO: Consider building a simple mock shipping service.

## 11. Notifications

- **FR-NTF-01** Customers receive transactional emails: account verification, password reset, order confirmation, shipping confirmation, cancellation, and refund etc.
- **FR-NTF-02** Merchant staff receive notifications for new orders, low stock, and payment failures.
- **FR-NTF-03** Tenants can customize notification templates and sender branding. (not included in v1)
- **FR-NTF-04** Tenants can subscribe webhooks to domain events (e.g. order created, order paid, fulfillment created, inventory changed).

## 12. Audit & History

- **FR-AUD-01** Every state-changing action by a staff user, customer, API key, or system process is recorded in an append-only audit log: actor, tenant, action, target resource, before/after values, timestamp, and request origin.
- **FR-AUD-02** Orders show a timeline of all events (placed, paid, edited, fulfilled, refunded, notes, etc.).
- **FR-AUD-03** Tenant admins can search and filter their tenant's audit log; platform admins can search across tenants.
- **FR-AUD-04** Audit entries cannot be edited or deleted through the product.

## 13. Platform Administration

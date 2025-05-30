# CMP Product Feed Field Specification (v0.1)

This document defines the field-level specification for product feeds in CMP, using schema.org-compatible syntax.

---

## 🎯 Top-Level Object: ItemList

The feed should be structured as an `ItemList` of `ListItem` objects. Each `ListItem` wraps a `Product` or `ProductGroup`.

```json
{
  "@context": "https://schema.org",
  "@type": "ItemList",
  "itemListElement": [ ... ]
}
```

---

## 📦 Product Fields (`@type: Product`)

| Field | Required | Type | Notes |
|-------|----------|------|-------|
| `@id` | ✅ | string | Unique product URI. Recommended: `urn:cmp:sku:{domain}:{sku}` |
| `sku` | ✅ | string | Unique SKU ID |
| `name` | ✅ | string | Product display name |
| `description` | ✅ | string | Short marketing description |
| `category` | ✅ | string | Category path, e.g. `"Pet Supplies > Dog Toys"` |
| `brand.name` | ✅ | string | Brand name |
| `offers.price` | ✅ | number | Selling price |
| `offers.priceCurrency` | ✅ | string | ISO 4217 currency code (e.g., `USD`) |
| `offers.availability` | ✅ | URL | Should use schema.org URLs (e.g. `https://schema.org/InStock`) |
| `offers.inventoryLevel.value` | ❌ | integer | Quantity available |
| `offers.priceValidUntil` | ❌ | datetime | Price expiration date |
| `additionalProperty` | ❌ | array | Variant-specific attributes (color, size, material, etc.) |

---

## 🧬 Variant Modeling

Use `ProductGroup` to represent families of products that vary by defined properties.

### ProductGroup

| Field | Required | Type | Notes |
|-------|----------|------|-------|
| `@type` | ✅ | `"ProductGroup"` | |
| `@id` | ✅ | string | Unique identifier for the product group |
| `name` | ✅ | string | Shared name |
| `description` | ✅ | string | Shared description |
| `productGroupID` | ✅ | string | Group ID |
| `variesBy` | ✅ | array of strings | Allowed: `["size", "color", "material", "denominations", ...]` |

### Product

| Field | Required | Notes |
|-------|----------|-------|
| `isVariantOf` | ✅ | Reference to `@id` of ProductGroup |
| `additionalProperty` | ✅ | Must define properties like size/color to differentiate |

---

## 🧰 Sample `additionalProperty`

```json
"additionalProperty": [
  { "@type": "PropertyValue", "name": "color", "value": "Red" },
  { "@type": "PropertyValue", "name": "size", "value": "32" }
]
```

---

## 🔍 Indexing Fields

Used for embedding and semantic search:

- `name`
- `description`
- `brand.name`
- `category`
- `additionalProperty[*].name` + `value`

---

## 🛠 Notes

- Use ISO 8601 format for all timestamps
- Use schema.org URLs for enums (e.g. `availability`)
- Avoid platform-specific keys (e.g. Shopify handles) unless namespaced under `"external"`

---

For more examples, see `/examples/sample.jsonld`

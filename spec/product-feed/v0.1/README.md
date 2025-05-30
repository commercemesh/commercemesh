# CMP Product Feed Specification (v0.1)

Welcome to the CMP Product Feed Specification. This document defines how brands and sellers can structure and publish their product data feeds in a format that is compatible with the Commerce Mesh Protocol (CMP), leveraging the widely adopted [schema.org](https://schema.org/) vocabulary.

---

## 📌 Overview

CMP uses structured product data to power AI-driven commerce experiences. Instead of storefronts, CMP-compatible discovery nodes ingest machine-readable feeds published by brands. Buyer agents query these discovery nodes rather than accessing the feeds directly.

To simplify adoption, CMP builds directly on `schema.org/Product` and `schema.org/ProductGroup`.

This specification documents:

- Which subset of schema.org is used
- Required and optional fields
- Conventions for product variants
- Feed file structure and hosting requirements
- How to shard large catalogs

---

## ✅ Schema Alignment

- Base Type: `schema.org/ItemList` with entries of `ListItem` wrapping `Product` or `ProductGroup`
- For variants: `schema.org/ProductGroup`
- No custom vocabulary introduced
- Additional attributes should use `additionalProperty` with `PropertyValue`

---

## 🔤 Required Fields

At minimum, each `Product` must contain:

```json
{
  "@context": "https://schema.org",
  "@type": "Product",
  "@id": "urn:cmp:sku:brand.com:SKU123",
  "sku": "SKU123",
  "name": "Product Name",
  "description": "Short description of product",
  "brand": { "@type": "Brand", "name": "Brand Name" },
  "category": "Product Category",
  "offers": {
    "@type": "Offer",
    "price": 12.99,
    "priceCurrency": "USD",
    "availability": "https://schema.org/InStock"
  }
}
```

---

## 🧬 Product Variants

Use `ProductGroup` to model variants:

- `ProductGroup` contains shared metadata and a `variesBy` array
- Each variant references the group via `isVariantOf`
- Use `additionalProperty` for differentiating values (e.g., color, size)

See `/examples/sample.jsonld` for a working sample.

---

## 🪵 Feed Hosting Requirements

Feeds must be publicly accessible JSON-LD files:

- Recommended path: `https://brand.com/.well-known/cmp/feed-000.json`
- Support sharded files named `feed-000.json`, `feed-001.json`, etc.
- Include a `feed-index.json` with:
  ```json
  { "version": "0.1", "shards": ["feed-000.json", "feed-001.json"] }
  ```

---

## 🔢 Sharding Strategy

CMP supports brands with thousands of SKUs.

- Use SHA1 of SKU mod N (e.g., 10) to determine file placement
- Example:
  ```python
  shard_id = int(hashlib.sha1(sku.encode()).hexdigest(), 16) % 10
  filename = f"feed-{shard_id:03d}.json"
  ```

---

## 🔁 Feed Updates

Brands should keep their feeds up to date.

- Feeds should be updated automatically whenever:
  - A product is created, updated, or deleted
  - A product's price changes
  - Inventory levels change

This can be achieved via webhook triggers, scheduled exports, or integration with your commerce platform's product lifecycle events.

Brands using other platforms can integrate via webhook or re-export tools.

---

## 🧠 Embeddings + Discovery

The following fields are used to compute product embeddings:

- `name`
- `description`
- `category`
- `brand.name`
- Variant differentiators from `additionalProperty`

---

## 🧪 Validation

To validate your feed:

- Use [validator.schema.org](https://validator.schema.org/)

---


## 🛡️ License

This spec is published as open documentation to enable broader adoption. It is versioned and maintained by CommerceMesh.

For issues or contributions, visit: [github.com/commercemesh](https://github.com/commercemesh)

---


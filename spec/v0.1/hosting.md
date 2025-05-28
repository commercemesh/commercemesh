# Feed Hosting Guidelines (v0.1)

This document outlines how brands should host and expose their CMP-compatible product feeds for consumption by discovery nodes.

---

## 🌐 Hosting Path

Feeds **must be publicly accessible JSON-LD** files and should follow a consistent, discoverable URL path:

### ✅ Recommended:

```
https://brand.com/.well-known/cmp/feed-000.json
```

- Use `.well-known/cmp/` as the base directory for CMP compatibility
- All discovery nodes will look for feeds at this path


---

## 🧩 Feed Index

When using sharded feeds, include a `index.json` file to list the available shards:

```json
{
  "version": "0.1",
  "shards": [
    "feed-000.json",
    "feed-001.json",
    "feed-002.json"
  ]
}
```

Place this file at:

```
https://brand.com/.well-known/cmp/index.json
```

---

## 🗂️ File Format

- Each `feed-xxx.json` file MUST be a valid `ItemList` using schema.org vocabulary
- Use UTF-8 encoding
- Recommended file size per shard: **under 5MB**

---

## 🧮 Sharding Strategy

Brands should shard their catalogs deterministically using SKU-based hashing:

```python
import hashlib

def get_feed_filename(sku, total_shards=10):
    shard_id = int(hashlib.sha1(sku.encode()).hexdigest(), 16) % total_shards
    return f"feed-{shard_id:03d}.json"
```

---

## 🚀 Hosting Tips

- Use a static site host (e.g. Netlify, Vercel, GitHub Pages, Cloudflare Pages)
- Automate feed regeneration and publishing via GitHub Actions or CI/CD
- Set proper cache-control headers to reflect feed freshness

---

## 🧪 Testing Your Feed

- Validate each file against `schema.org` using [validator.schema.org](https://validator.schema.org/)
- Use `curl` or browser DevTools to confirm HTTP 200 and correct `Content-Type`

---

## ❗Common Pitfalls

- File not publicly accessible (check HTTPS + CORS)
- Incorrect MIME type (should be `application/ld+json`)
- Feeds only available via JavaScript-rendered routes (not supported)

---

Need help hosting your feed? Contact `specs@commercemesh.ai`

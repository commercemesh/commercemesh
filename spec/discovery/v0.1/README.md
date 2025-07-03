# Commerce Mesh – Discovery Protocol (v0.1)

The Discovery node is **the search layer** of Commerce Mesh: a neutral, open API that lets any front-end, marketplace, chatbot, or autonomous agent find products, inspect rich offers, and emit engagement signals—**without** locking into one retail platform.

> **TL;DR**  
> *Search in, JSON-LD out.  
> Live prices & stock via SSE.  
> Webhooks for telemetry.*  

---

## Why another “product search” spec?

1. **Multiplayer commerce** – One merchant’s PDP is another app’s data source. We need a lingua franca, not ten proprietary feeds.  
2. **LLM-native** – GPT / Claude agents want structured “tools” and prompt templates, not scraped HTML.  
3. **Edge-cache-able** – Static catalogue shards continue to serve when the API is rate-limited or offline.  
4. **Incremental adoption** – REST/JSON first; realtime SSE & outbound webhooks optional.

---

## Surface area at a glance

| Layer                         | Spec file               | Purpose                                          |
|-------------------------------|-------------------------|--------------------------------------------------|
| **REST / HTTP JSON**          | `openapi.yaml`          | Human & machine friendly; great for curl/CDN     |
| **MCP (Model-Context)**       | `mcp.yaml`              | Declares tools/resources for LLMs                |
| **Static JSON-LD catalogue**  | `examples/catalogue/*`  | Crawlable, ≤2 MB shards, perfect for bulk sync   |
| **SSE push channel**          | `stream/sse.md`         | Millisecond-latency price/stock changes          |
| **Outbound webhooks**         | `webhooks.yaml`         | Merchants receive `offer.viewed`, `basket.*`, …  |

---


# Commerce Mesh – Discovery Protocol (v0.1)

The Discovery node is **the search layer** of Commerce Mesh: a neutral, open API that lets any front-end, marketplace, chatbot, or autonomous agent find products, inspect rich offers, and emit engagement signals—**without** locking into one retail platform.

> **TL;DR**  
> *Search query, JSON-LD reesponse.  
> Live prices & inventory via SSE.  
> Webhooks for telemetry.*  

---

## Why another “product search” spec?

1. **Multiplayer commerce** – One merchant’s PDP is another app’s data source. We need a lingua franca, not ten proprietary feeds.  
2. **LLM-native** – GPT / Claude agents want structured “tools” and prompt templates, not scraped HTML.  
3. **Edge-cache-able** – Static catalogue shards continue to serve when the API is rate-limited or offline.  
4. **Incremental adoption** – REST/JSON first; realtime SSE & outbound webhooks optional.

---

## Surface area at a glance

| Layer                         | Spec file               | Purpose                                          |
|-------------------------------|-------------------------|--------------------------------------------------|
| **REST / HTTP JSON**          | `openapi.yaml`          | Human & machine friendly; great for curl/CDN     |
| **MCP (Model-Context)**       | `mcp.yaml`              | Declares tools/resources for LLMs                |
| **Static JSON-LD catalogue**  | `examples/catalogue/*`  | Crawlable, ≤2 MB shards, perfect for bulk sync   |
| **SSE push channel**          | `stream/sse.md`         | Millisecond-latency price/stock changes          |
| **Outbound webhooks**         | `webhooks.yaml`         | Merchants receive `offer.viewed`, `basket.*`, …  |

---

## Directory layout



## Example: faceted search
```sh
curl -G 'https://api.my-discovery-node.com/products' \
     --data-urlencode 'q=organic coffee beans under $25' \
     --data-urlencode 'filter={"roast":"Dark Roast"}'

```

Respomse (abridged)
```json
{
  "products": [
    {
      "@context": "https://schema.org",
      "@type": "Product",
      "skuUrn": "urn:cmp:sku:bluebottle:obsidian-12oz",
      "name": "Obsidian Blend – 12oz",
      "image": "https://cdn…/obsidian.jpg",
      "offers": [
        { "offerId": "off_123", "price": 18.00, "priceCurrency": "USD" }
      ]
    },
    …
  ],
  "nextPageToken": "eyJpc29mZiI6M30"   // opaque
}

```


## Versioning policy
**MAJOR** – breaking field/schema changes  
**MINOR** – additive fields or endpoints (backwards compatible)  
**PATCH** – doc clarifications, bug-fixes, no contract impact  

v0.1 targets functional completeness; expect one more MINOR before 1.0.  


## Contributing

Pull requests are welcome—please:

1. **Open an issue** describing the gap or change.
2. **Update** the relevant JSON-Schema / OpenAPI file **and** provide example fixtures.
3. **Run** `make validate` locally; CI must stay ✅.

Maintainers reserve the right to defer large breaking changes until the next **MAJOR** release.

---

## Changelog

| Version | Date       | Notes                                   |
|---------|------------|-----------------------------------------|
| 0.1.0   | 2025-07-03 | First public draft; REST + MCP + SSE    |

---

## License

Apache 2.0 — free to use, fork, and implement under the same license.

---

## Contact & Community

- **Spec discussion:** GitHub Issues (`spec/discovery` label)  
- **Slack:** `#cmp-discovery` on the Commerce Mesh workspace  
- **Security:** support@commercemesh.ai (PGP key in repo root)  

Help us make commerce truly *composable*—file issues, suggest schemas, or share your production war stories!

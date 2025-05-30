# Commerce Mesh Protocol (CMP) Registry

Welcome to the CMP Registry — the decentralized index for brands and discovery nodes participating in the Commerce Mesh Protocol.

## What is This?
This registry defines the **minimum viable data format** to onboard an **organization (brand or seller)** into the CMP network. It adheres to the [schema.org](https://schema.org/Organization) standard and is extended with `cmp:` fields where necessary.

Participants are encouraged to provide full [schema.org](https://schema.org) payloads. However, **only a specific set of fields are required** to participate in CMP.


## 🧠 How This File Is Used
Each entry in the brand registry acts as a **decentralized profile** for a participating organization. Discovery nodes use this file to:
- **Identify** the organization and its brands
- **Locate** the canonical product feed endpoint
- **Filter** relevant brands by category, industry, or URNs

This file enables AI agents, marketplaces, and infrastructure partners to programmatically understand and transact with each brand without prior integration.



## 📦 Required Fields for Organizations (Brands/Sellers)

Minimum required fields include:
- `@type`: Must be `Organization`
- `name`: Name of the organization
- `url`: Canonical URL of the organization
- `logo`: URL to organization logo
- `description`: Short description of the organization
- `identifier`: A nested object with a URN value under `cmp:orgId`
- `brand`: A nested `Brand` object with `name`, `logo`, and `identifier` (URN under `cmp:brandId`)
- `cmp:category`: One or more categories the organization operates in
- `cmp:productFeed.url`: Link to a well-known feed index JSON

## 📦 Required Fields for Discovery Nodes
- `@type`: Must be `cmp:DiscoveryNode`
- `name`: Name of the discovery node
- `url`: Canonical service URL of the discovery node
- `cmp:description`: Short description of service scope or vertical focus
- `cmp:coverage`: List of `cmp:brandId` values or category paths covered
- `cmp:maintainer`: Name or organization running the node

## ✅ Optional but Recommended
- `sameAs`: Array of social/profile links

## 🧩 Extending with Full Schema.org
You are welcome to include **full schema.org payloads** if your infrastructure already supports it. CMP will index required fields, and agents can optionally parse extended metadata.


## 📬 Questions?
Open an issue 

---

# CMP Registry Field Spec

## 🔐 Required Fields — Organization
| Field | Type | Description |
| --- | --- | --- |
| `@type` | `string` | Must be `Organization` |
| `name` | `string` | Name of the organization |
| `url` | `string` | Canonical URL to the organization |
| `logo` | `string` | URL to organization logo |
| `description` | `string` | Short description of the organization |
| `identifier.value` | `string` | URN-style unique identifier (e.g., `cmp:orgId`) |
| `brand.name` | `string` | Brand name under the organization |
| `brand.logo` | `string` | Logo URL for the brand |
| `brand.identifier.value` | `string` | URN-style brand identifier (e.g., `cmp:brandId`) |
| `cmp:category` | `array` | List of categories the brand operates in |
| `cmp:productFeed.url` | `string` | URL to feed index JSON |

## 🔐 Required Fields — Discovery Node
| Field | Type | Description |
| --- | --- | --- |
| `@type` | `string` | Must be `cmp:DiscoveryNode` |
| `name` | `string` | Name of the discovery node |
| `url` | `string` | Canonical service URL of the discovery node |
| `cmp:description` | `string` | Description of the node’s purpose or scope |
| `cmp:coverage` | `array` | List of `cmp:brandId` values or categories served |
| `cmp:maintainer` | `string` | Entity maintaining the discovery node |

## ✅ Optional Fields
| Field | Type | Description |
| --- | --- | --- |
| `sameAs` | `array` | List of social media or web profile URLs |

## 🧩 Full Schema.org Support
If you already use schema.org `Organization`, `Product`, `DataFeed`, or `Service`, simply include your full JSON-LD. CMP extractors will parse required and extended fields.

See Example - /examples/brand-registry.jsonld  
Schema - /examples/brand-registry-schema.jsonlod

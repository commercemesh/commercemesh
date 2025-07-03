# CMP Brand Registry

Welcome to the **Commerce Mesh Protocol (CMP) Brand Registry** — a decentralized, open registry of organizations participating in the CMP ecosystem.

## 🌍 What is This?

This registry serves as the **canonical directory** of brands that have joined the Commerce Mesh Protocol. Each entry provides essential metadata that enables:

- **AI agents** to discover and interact with brands programmatically
- **Discovery nodes** to index and serve product catalogs
- **Infrastructure partners** to build interoperable commerce tools
- **Developers** to integrate with CMP-compatible organizations

## 📋 Registry Structure

The registry is maintained as a simple JSON array in [`brands.json`](./brands.json), where each element represents an organization following the [schema.org/Organization](https://schema.org/Organization) standard with CMP-specific extensions.

```json
[
 {
   "@context": {
     "schema": "https://schema.org",
     "cmp": "https://schema.commercemesh.ai/ns#"
   },
   "@type": "Organization",
   "name": "Example Brand",
   "description": "Brief description of the organization",
   "url": "https://example.com",
   "identifier": {
     "@type": "PropertyValue",
     "propertyID": "cmp:orgId",
     "value": "urn:cmp:orgid:123e4567-e89b-12d3-a456-426614174000"
   }
   // ... additional fields
 }
]
```



## 🔑 Required Fields

To register your organization, provide these minimum required fields:

| Field | Type | Description |
|-------|------|-------------|
| `@type` | string | Must be `"Organization"` |
| `name` | string | Your organization's display name |
| `description` | string | Brief description of your organization |
| `url` | string | Your canonical domain URL |
| `logo` | string | URL to your organization's logo |
| `brand` | object | Brand information with name, logo, and identifier |
| `cmp:category` | array | Categories your organization operates in |
| `cmp:productFeed` | object | Link to your product feed index |

**Note:** You must generate and include your URN identifiers when submitting your entry.

## 🔧 How to Register

### 1. Generate Your URN Identifiers

Before submitting your entry, generate your unique URN identifiers using the CMP namespace:

### CMP_NAMESPECE = **4c2d9653-e971-4093-8d5b-82da447c2e85**

Example:
```javascript
// javascript
import { v5 as uuid } from 'uuid';

const CMP_NAMESPACE = '4c2d9653-e971-4093-8d5b-82da447c2e85'; 
                       
const generateOrgId = (domain) => {
  const uuid = uuid(domain, CMP_NAMESPACE);
  return `urn:cmp:orgid:${uuid}`;
};
```

```python
# python
import uuid

CMP_NAMESPACE = uuid.UUID("4c2d9653-e971-4093-8d5b-82da447c2e85")

def generate_org_id(domain):
    urn_id = uuid.uuid5(CMP_NAMESPACE,  domain)
    return f"urn:cmp:orgid:{urn_id}"

```

### 2. Fork & Add Your Entry

1. **Fork** this repository
2. **Edit** [`brands.json`](./brands.json) 
3. **Add** your organization's entry to the array (including your generated URN identifiers)
4. **Submit** a pull request

### 3. Review & Merge

Your pull request will be reviewed for:
- Valid JSON format
- Correct URN generation
- Complete required fields
- Working URLs (basic check)

## 🆔 URN Identifier System

CMP uses **deterministic URN generation** to ensure consistent, verifiable identifiers:

### Organization URNs
urn:cmp:orgid:123e4567-e89b-12d3-a456-426614174000

### Brand URNs
urn:cmp:brandid:987fcdeb-51a2-43d1-b456-789012345678

> If your organization and brand is the same, use the same URN

### How URNs are Generated

URNs are created using **UUID v5** with a public CMP namespace:

### Benefits of this approach:

- **Deterministic:** Same domain always generates the same URN
- **Verifiable:** Anyone can independently verify your URN is correct
- **Collision-resistant:** Cryptographically unique across organizations
- **Transparent:** No hidden or proprietary identifier generation


## 🌐 API Access

### Get All Organizations

```javascript
const organizations = await fetch(
  'https://raw.githubusercontent.com/commercemesh/commercemesh/main/registry/brands.json'
).then(r => r.json());
```

### Search by Category
```javascript
const bookPublishers = organizations.filter(org => 
  org['cmp:category']?.includes('books')
);
```

### Find by Domain
```javascript

const brand = organizations.find(org => 
  org.url.includes('yoursite.com')
);
```

### CDN Access (Faster)
```javascript

const organizations = await fetch(
  'https://cdn.jsdelivr.net/gh/commercemesh/commercemesh@main/registry/brands.json'
).then(r => r.json());

```


## 📖 Examples
[See](../spec/brand-registry/v0.1/schema/example.jsonld)  for:

- Complete organization entry examples
- JSON schema definitions
- Validation samples

## 🛡️ Verification Requirements
Current Status: Manual Review
Currently, registry entries undergo manual review for basic quality checks.

**Coming Soon:**

 - Domain ownership verification via DNS TXT record or well-known file
 - Automated product feed validation at specified cmp:productFeed.url
 - Enhanced JSON-LD validation with schema.org compliance checks
 - Endpoint reachability testing for all submitted URLs


## 🔄 Keeping Your Entry Updated
Your registry entry should be updated when:

- Your organization's basic information changes
- Your product feed URL changes
- Your brand information is updated
- Your logo or social media links change

Submit a new pull request with your updates following the same process.

## 📞 Support

Issues: Open a GitHub issue
Discussions: Join the conversation
Specification: See ../spec/brand-registry/ for detailed documentation

## 📜 License

This registry is open source under the [MIT License](../LICENSE). Participation in the registry implies agreement to make your basic organization metadata publicly available.

---

> **Building the future of commerce, one brand at a time.**  
> Protocols, not platforms. 🚀
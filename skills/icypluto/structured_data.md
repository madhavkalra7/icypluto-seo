# Structured Schema Optimization Guide

Search engine engines and AI assistants use Schema.org configurations to resolve entity context (resolving brand details, products, founders, official websites, and social links).

This guide provides instructions for you (the AI Agent) to dynamically construct and inject schemas depending on the website's specific niche.

---

## 🏗️ Dynamic Schema Generation Blueprint

### Step 1: Detect the Business Category
Check the site's copy and metadata to classify the entity:
1.  **Corporate/Brand Website**: Use `Organization` + `WebSite` schemas.
2.  **SaaS/Product Website**: Use `Organization` + `SoftwareApplication` / `Product` schemas.
3.  **Local Service Business**: Use `LocalBusiness` + `Service` schemas.

### Step 2: Construct the Structured JSON-LD Payload
Write the JSON block using the relevant structure from the templates below, substituting fields dynamically using the resolved domain and brand variants.

#### Niche Template A: Organization & Website
```json
{
  "@context": "https://schema.org",
  "@graph": [
    {
      "@type": "Organization",
      "@id": "{{RESOLVED_DOMAIN}}/#organization",
      "name": "{{BRAND_NAME}}",
      "url": "{{RESOLVED_DOMAIN}}",
      "logo": "{{RESOLVED_DOMAIN}}/logo.png",
      "sameAs": [
        "https://twitter.com/{{TWITTER_HANDLE}}",
        "https://linkedin.com/company/{{LINKEDIN_SLUG}}"
      ],
      "description": "{{BRAND_DESCRIPTION}}"
    },
    {
      "@type": "WebSite",
      "@id": "{{RESOLVED_DOMAIN}}/#website",
      "url": "{{RESOLVED_DOMAIN}}",
      "name": "{{BRAND_NAME}}",
      "publisher": {
        "@id": "{{RESOLVED_DOMAIN}}/#organization"
      }
    }
  ]
}
```

#### Niche Template B: SoftwareApplication (SaaS)
If the site represents a software product, append the following entity:
```json
{
  "@context": "https://schema.org",
  "@type": "SoftwareApplication",
  "name": "{{PRODUCT_NAME}}",
  "operatingSystem": "All",
  "applicationCategory": "BusinessApplication",
  "offers": {
    "@type": "Offer",
    "price": "0.00",
    "priceCurrency": "USD"
  }
}
```

---

## 🛠️ Step-by-Step Injection Instructions

1.  **Resolve Domain and Brand Name**: Extract these from codebase configs as instructed in [SKILL.md](./SKILL.md).
2.  **Locate Root Layout/Template**:
    *   *Next.js*: Find the root layout (`app/layout.tsx` or `pages/_document.tsx`).
    *   *React/Vite/Static HTML*: Find the main template or `index.html` head tags.
3.  **Inject the Script Block**:
    *   Insert the JSON-LD payload wrapped in `<script type="application/ld+json">`.
    *   *Critical Performance Rule*: Ensure the structured data script does not block initial load. In React/Next.js frameworks, wrap the injection using dangerouslySetInnerHTML or load it inside page layouts rather than heavy blocking JS components.
4.  **Validate Schema Integrity**:
    *   Verify that all JSON keys are properly double-quoted.
    *   Verify that no trailing commas are present.
    *   Verify that all URLs in the schema use absolute paths (with the correct protocol: `https://`).

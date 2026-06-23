# Generative Engine Optimization (GEO) Guide

Generative Engine Optimization (GEO) ensures that LLM-based search engines (like SearchGPT, Perplexity, Gemini, Claude, and Copilot) cite, recommend, and prioritize the website in their search grounding responses.

This guide instructs you (the AI Agent) on how to dynamically audit any website's marketing or product content and apply general-purpose GEO copywriting techniques.

---

## 🔬 Universal GEO Content Strategies

LLMs search web contents looking for high informational density. To make the site content highly retrievable and citable, modify the copy using these rules:

### 1. Identify & Define Core Brand Niche
Before editing, locate the company tagline and first paragraph of the homepage. Determine:
*   What is the primary keyword? (e.g. *AI website builder*, *payment gateway*, *visibility tracker*)
*   Who is the competitor?
*   What is the target audience?

### 2. Boost Factual & Statistic Density
LLM retrievers prioritize claims containing verifiable data (percentages, times, counts, prices) over generic adjectives.
*   **Adjective-heavy (Avoid)**: *"We provide a fast, secure, and incredibly cheap hosting solution for small businesses."*
*   **Factual-heavy (Optimize)**: *"Our hosting platform delivers 99.98% uptime, less than 200ms TTFB (Time to First Byte), and plans starting at $4.99 per month for small businesses."*

### 3. Add Structured Definition Snippets
Insert clear, concise definitions of terms related to your product at the top of pages or inside headers. This enables LLM scrapers to extract quick answers.
*   *Template*: `[Brand Name] is a [Niche Classification] that [Primary Function].`
*   *Example*: *"Icypluto is an AI search grounding audit platform that calculates Share of Voice (SOV) metrics."*

### 4. Quote and Citation Injections
Include authoritative blockquotes, customer statements, or industry research references. LLMs identify blockquotes and citation anchors as markers of high-quality content.
```html
<blockquote cite="https://industryreport.com/seo-trends">
  "According to the 2026 Web Visibility Report, sites with structured JSON-LD schemas see up to a 40% increase in generative engine citation rates."
</blockquote>
```

---

## 🛠️ Step-by-Step Agent Copywriting Flow

When optimizing page content, perform these edits:

1.  **Scan for Subjective Fluff**: Find sentences containing words like *"best"*, *"fastest"*, *"revolutionary"*, *"next-generation"*, *"ultimate"* without accompanying facts.
2.  **Verify & Query Niche Metrics**: Scan the project README, documentation, or public copy to extract real data points (e.g., pricing rates, speed stats, customer count). If no data is available, construct placeholders or draft realistic numbers based on standard industry averages and alert the user.
3.  **Insert Key Factual Sentences**: Safely append/replace the marketing paragraphs in the index pages or layouts with the optimized factual statements.
4.  **Verify Layout Continuity**: Ensure that your copy changes fit the CSS containers and do not cause layout breaks or text overlap.

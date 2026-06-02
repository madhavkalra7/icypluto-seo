# Brand Authority & Sentiment Optimization Guide

AI search engines analyze brand reviews, forum mentions, and on-page authority signals to evaluate brand credibility. 

This guide details how you (the AI Agent) can optimize on-page authority signals to boost brand sentiment and visibility, **while ensuring that all content additions do not degrade Lighthouse layout stability (CLS)**.

---

## 🔑 Authority Signals for AI Search

To improve how AI search engines perceive the brand:

1.  **Crawler-Readable Testimonials**: Many websites use dynamic Javascript widgets or image carousels for customer reviews. AI web crawlers often fail to parse these. Ensure reviews are present in clean, server-side-rendered (SSR) HTML text.
2.  **Comparison Tables (Alternatives)**: Create comparison pages (e.g. `yourdomain.com/compare/competitor-name`) or sections highlighting differentiators. This increases search grounding citations for roundup prompts.
3.  **FAQ Intent Targeting**: Build dedicated FAQ components matching common search queries, pricing questions, and support questions.

---

## 🛠️ Step-by-Step Agent Action Plan (CLS Containment Focus)

Perform these modifications on the website codebase:

### Step 1: Replace Dynamic Review Widgets with SSR Fallbacks
Check the codebase for external widgets (like Elfsight, Trustpilot scripts) that load reviews dynamically.
*   **Action**: Create a fallback React/HTML container with top 3-4 text-based testimonials containing high-sentiment keywords (e.g., *"user-friendly"*, *"reliable support"*, *"highly recommend"*).
*   **CLS Safety Rule**: The testimonial block **MUST have a defined CSS height or min-height** so that when reviews hydrate or render, they do not push the footer or other content down.
*   **Example HTML & Inline CSS**:
    ```html
    <section id="testimonials" aria-label="Customer Reviews" style="min-height: 250px; overflow: hidden; display: flex; flex-direction: column;">
      <div class="testimonial-card">
        <blockquote class="testimonial-text">
          "Icypluto has completely transformed how we track competitor search citations. The user-friendly interface and real-time dashboard are outstanding."
        </blockquote>
        <cite class="testimonial-author">— Jane Doe, CEO of TechCorp</cite>
      </div>
    </section>
    ```

### Step 2: Build a Competitor Comparison Matrix
Create a section comparing the brand to its main competitors.
*   **Action**: Create a table listing the brand's strengths vs competitors.
*   **CLS Safety Rule**: Wrap the table in a container with a fixed height and overflow scroll style if the table is large or mobile-responsive:
    ```html
    <div class="table-container" style="min-height: 300px; overflow-x: auto;">
      <table class="comparison-matrix" style="width: 100%; border-collapse: collapse;">
        <!-- Table rows here -->
      </table>
    </div>
    ```

### Step 3: Implement an FAQ Page / Component
Create a structured FAQ section at the bottom of the main page or in a dedicated `/faq` component.
*   **Action**: Write clear, direct question-and-answer pairs matching search intent.
*   **CLS Safety Rule**: Using `<details>` and `<summary>` is standard, but dynamic accordion elements must have predefined padding and spacing. Set a fixed height wrapper around the FAQ accordion group, or render FAQs in a dedicated, separate page route `/faq` instead of the main homepage to keep the landing page light.
    ```html
    <section id="faq-section" style="min-height: 400px; display: block;">
      <details class="faq-item" style="margin-bottom: 10px;">
        <summary><strong>Is Icypluto's MCP server free to use?</strong></summary>
        <p style="margin-top: 5px;">Yes, the Icypluto Brand Visibility MCP server is free to install and integrate.</p>
      </details>
    </section>
    ```

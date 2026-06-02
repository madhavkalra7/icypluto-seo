# Performance & Accessibility Optimization Guide

High-performing websites load quickly and are accessible. Lighthouse audits measure:
*   **Performance**: Focuses on load speed (FCP/LCP), main-thread work, and layout stability (CLS).
*   **Accessibility**: Focuses on assistive accessibility cues (ARIA labels, semantic elements).

This guide instructs you (the AI Agent) on how to find and resolve common Performance and Accessibility bottlenecks within any website codebase, with a **strict focus on preventing layout shifts (CLS)**.

---

## ⚡ Performance Optimization (FCP, LCP, CLS, TBT)

### 1. Strict Cumulative Layout Shift (CLS) Containment Rules
Layout shifts occur when images, third-party widgets, or dynamic elements load asynchronously and push other elements around. **CLS must be kept under 0.1.**

When injecting visible UI elements (like FAQs, reviews, or comparison tables), you **MUST** follow these styling containment directives:
*   **Explicit Min-Height Skeletons**: If injecting a dynamic or hydrated client-side component (e.g. an accordion list or review feed), wrap the component in a container that has a defined `min-height` or layout skeleton placeholder.
    ```html
    <!-- Avoid: Will expand when reviews hydrate, causing a massive CLS jump -->
    <div id="reviews-block">
      <ReviewsComponent />
    </div>

    <!-- Correct: Set a fixed min-height corresponding to the expected rendered content -->
    <div id="reviews-block" style="min-height: 350px; overflow: hidden;">
      <ReviewsComponent />
    </div>
    ```
*   **Prevent Font Layout Shifts**: Ensure that external custom web fonts are configured with `font-display: swap` in the stylesheet or header to prevent Invisible Text Shift (FOIT).
*   **Absolute Dimensions on Images**: Always supply explicit `width` and `height` attributes to `<img>` tags, or use CSS aspect-ratio properties.

### 2. Render-Blocking & Main-Thread Optimization
Scripts in the `<head>` delay page rendering while being downloaded and parsed, increasing Total Blocking Time (TBT).
*   **Fix**: Append `defer` or `async` tags to non-critical external scripts. Use preconnect directives for critical third-party fonts and APIs.
*   **Dynamic Component Loading (React/Next.js)**: If injecting an interactive widget (such as a comparison chart or accordion FAQ), load it dynamically using `next/dynamic` or `React.lazy` to keep the main bundle lightweight:
    ```tsx
    import dynamic from 'next/dynamic';
    
    // Dynamic import to defer main-thread parsing until interaction
    const FAQAccordion = dynamic(() => import('./FAQAccordion'), {
      loading: () => <div style={{ height: '300px' }}>Loading...</div>,
      ssr: false
    });
    ```

---

## ♿ Accessibility Optimization

### 1. Accessible Names for Interactive Elements
*   **Audit Warn**: *"Buttons do not have an accessible name."*
*   **Fix**: Inject an `aria-label` attribute or add a screen-reader-only `<span>` inside the button.
*   **Code Pattern**:
    ```html
    <!-- Avoid -->
    <button class="icon-btn">
      <svg>...</svg>
    </button>

    <!-- Correct -->
    <button class="icon-btn" aria-label="Toggle Navigation Menu">
      <svg>...</svg>
    </button>
    ```

### 2. Image Alternative Text
*   **Audit Warn**: *"Image elements do not have [alt] attributes."*
*   **Fix**: Walk through every `<img>` element in the templates and ensure there is an descriptive `alt="..."` value.

---

## 🛠️ Step-by-Step Codebase Optimization Flow

1.  **Crawl Components**: Scan the workspace codebase to locate `<img>` and `<button>` tags.
2.  **Add Size Attributes**: For every image, verify that `width` and `height` properties exist. If missing, look at the styling classes or guess the dimensions, adding explicit values.
3.  **Audit Empty Buttons**: Find buttons containing only icons or SVGs and add a clear descriptive `aria-label` matching their visual context (e.g. `aria-label="Close modal"`).
4.  **Defer Scripts**: Identify custom script tags and add `async` or `defer` where safe to prevent render-blocking warnings.
5.  **Strict Height Constraint Check**: Inspect all files you edited or created. Ensure any newly added UI element contains inline or class-based CSS that enforces its vertical layout boundaries to prevent layout shifts.

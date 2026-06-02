# Performance & Accessibility Optimization Guide

High-performing websites load quickly and are accessible. This guide details how you (the AI Agent) can programmatically parse diagnostics from `./lighthouse-report.json` and map specific failures directly to their matching files in the codebase.

---

## 🔬 Parsing Lighthouse JSON Diagnostics

When parsing `lighthouse-report.json`, inspect the `audits` object. Look for the following audit IDs, extract the offending elements, and locate them in the codebase:

### 1. Cumulative Layout Shift (CLS)
*   **Audit ID**: `cumulative-layout-shift`
*   **Target Score**: `details.overallSavingsMs === 0` (or CLS value `< 0.1`)
*   **How to Parse**: Check `audits['cumulative-layout-shift'].details.items`. Each item has a `node` property representing the HTML element that shifted.
    *   *JSON path*: `details.items[].node.nodeLabel` (e.g. `img.responsive-img`, `div#faq-section`)
*   **Action**: 
    1. Search the codebase for the element class, ID, or tag.
    2. Add explicit styling containment:
       - **Images**: Add `width="..." height="..."` attributes or inline `style="aspect-ratio: ..."` styles.
       - **Dynamic Containers**: Add a wrapping `style="min-height: Xpx; overflow: hidden;"` to allocate space before child elements render.

### 2. Render-Blocking Resources
*   **Audit ID**: `render-blocking-resources`
*   **How to Parse**: Check `audits['render-blocking-resources'].details.items`.
    *   *JSON path*: `details.items[].url` (lists blocking stylesheet/JS URLs).
*   **Action**:
    1. Search the codebase for the matching script or stylesheet import.
    2. Make script loading non-blocking:
       - Add `async` or `defer` to script tags.
       - For Next.js, use `next/script` with a non-blocking `strategy`.
       - Preconnect fonts and APIs using `<link rel="preconnect" href="...">`.

### 3. Buttons Without Accessible Names
*   **Audit ID**: `button-name`
*   **How to Parse**: Check `audits['button-name'].details.items`.
    *   *JSON path*: `details.items[].node.nodeLabel` (identifies empty buttons).
*   **Action**:
    1. Search the codebase for the button node.
    2. Inject an `aria-label` attribute (e.g. `aria-label="Submit Menu"`) or insert a hidden `<span>` inside the button (e.g., `<span class="sr-only">Close</span>`).

### 4. Images Missing Alt Text
*   **Audit ID**: `image-alt`
*   **How to Parse**: Check `audits['image-alt'].details.items`.
    *   *JSON path*: `details.items[].node.nodeLabel` (identifies un-labeled images).
*   **Action**:
    1. Search the codebase for the target `<img>` nodes.
    2. Add descriptive, context-specific `alt="..."` text attributes.

---

## ⚡ Main-Thread & Hydration Optimization

If the audit reports heavy main-thread work or hydration delays:
1.  **Dynamic Import**: If a component (like a comparison table, dynamic chart, or list) uses client-side hooks, wrap it using `next/dynamic` or `React.lazy` to defer load times.
2.  **Server Response Times**: If `server-response-time` is flagged (TTFB > 600ms), check API routes in the codebase. Verify that heavy database requests are cached or that standard page routing uses static/ISR rendering profiles rather than dynamic blocking operations.

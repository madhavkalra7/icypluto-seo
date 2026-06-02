# Performance & Accessibility Optimization Guide

High-performing websites load quickly and are accessible to all users. Lighthouse audits measure:
*   **Performance**: Focuses on load speed (FCP/LCP) and layout stability (CLS).
*   **Accessibility**: Focuses on assistive accessibility cues (ARIA labels, semantic elements).

This guide instructs you (the AI Agent) on how to find and resolve common Performance and Accessibility bottlenecks within any website codebase.

---

## ⚡ Performance Optimization (FCP, LCP, CLS)

### 1. Cumulative Layout Shift (CLS) Fixes
Layout shifts occur when images or widgets load and push other elements around.
*   **Audit Warn**: *"Image elements do not have explicit width and height."*
*   **Fix**: Always supply explicit `width` and `height` attributes to `<img>` tags, or use CSS aspect-ratio properties.
*   **Code Pattern**:
    ```html
    <!-- Avoid -->
    <img src="/banner.jpg" alt="Company Banner" class="responsive-img" />

    <!-- Correct -->
    <img src="/banner.jpg" alt="Company Banner" width="1200" height="630" class="responsive-img" />
    ```

### 2. Render-Blocking Request Fixes
Scripts in the `<head>` delay page rendering while being downloaded and parsed.
*   **Audit Warn**: *"Render-blocking resources, particularly CSS and external scripts, delay rendering."*
*   **Fix**: Append `defer` or `async` tags to non-critical external scripts. Use preconnect directives for critical third-party fonts and APIs.
*   **Code Pattern**:
    ```html
    <!-- Avoid -->
    <script src="https://analytics.com/tracker.js"></script>

    <!-- Correct -->
    <script src="https://analytics.com/tracker.js" async></script>
    <link rel="preconnect" href="https://analytics.com" />
    ```

---

## ♿ Accessibility Optimization

### 1. Accessible Names for Interactive Elements
Screen readers cannot read icon-only buttons (like settings gears, close buttons, menu toggles) without explicit labels.
*   **Audit Warn**: *"Buttons do not have an accessible name."*
*   **Fix**: Inject an `aria-label` attribute or add a screen-reader-only `<span>` inside the button.
*   **Code Pattern**:
    ```html
    <!-- Avoid -->
    <button class="icon-btn">
      <svg>...</svg>
    </button>

    <!-- Correct Option A: aria-label -->
    <button class="icon-btn" aria-label="Toggle Navigation Menu">
      <svg>...</svg>
    </button>

    <!-- Correct Option B: Screen Reader Text -->
    <button class="icon-btn">
      <svg>...</svg>
      <span class="sr-only">Toggle Menu</span>
    </button>
    ```

### 2. Image Alternative Text
*   **Audit Warn**: *"Image elements do not have [alt] attributes."*
*   **Fix**: Walk through every `<img>` element in the templates and ensure there is an descriptive `alt="..."` value. Use `alt=""` only for purely decorative divider images.

---

## 🛠️ Step-by-Step Codebase Optimization Flow

1.  **Crawl Components**: Scan the workspace codebase (React JSX, Next.js page files, Vue templates, or HTML files) to locate `<img>` and `<button>` tags.
2.  **Add Size Attributes**: For every image, check if `width` and `height` properties exist. If missing, look at the styling classes or guess the dimensions, adding explicit values.
3.  **Audit Empty Buttons**: Find buttons containing only icons or SVGs and add a clear descriptive `aria-label` matching their visual context (e.g. `aria-label="Close modal"`).
4.  **Defer Scripts**: Identify custom script tags and add `async` or `defer` where safe to prevent render-blocking warnings.

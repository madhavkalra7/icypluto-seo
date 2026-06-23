# Technical SEO & Site Structure Guide

To achieve maximum SEO and GEO scores, a website's crawling files and tag structures must be syntactically valid and semantic. 

This guide details how you (the AI Agent) can:
1.  Verify/correct `robots.txt` syntax.
2.  Dynamically resolve the absolute domain name.
3.  **Generate a basic, valid sitemap.xml** if it is missing in the workspace.
4.  Optimize page-level title tags and heading hierarchies.

---

## 🛠️ Step-by-Step Codebase Optimization

### Step 1: Detect/Resolve Absolute Domain URL
Scan config files or environment files (as outlined in [SKILL.md](./SKILL.md)) to extract the site's deployment URL (e.g. `https://mywebsite.com`). 
*   *Crucial Rule*: Do not use placeholders or write relative URLs. If no domain is defined, prompt the user for their site domain or read it from git configurations (e.g. `git remote get-url origin`).

### Step 2: Validate or Generate `robots.txt`
1.  Locate `robots.txt` in the root or public folder. If it doesn't exist, create it.
2.  Verify the `Sitemap` path.
    *   **INCORRECT (SEO Penalty)**: `Sitemap: /sitemap.xml`
    *   **CORRECT (SEO Pass)**: `Sitemap: https://yourdomain.com/sitemap.xml`
3.  Ensure robots.txt allows standard AI agents to crawl the website:
    ```txt
    User-agent: *
    Allow: /

    # Ensure search-grounding and AI bots are permitted
    User-agent: GPTBot
    Allow: /
    User-agent: ClaudeBot
    Allow: /
    User-agent: PerplexityBot
    Allow: /
    User-agent: OAI-SearchBot
    Allow: /

    Sitemap: https://yourdomain.com/sitemap.xml
    ```

### Step 3: Generate `sitemap.xml` (If Missing)
If a sitemap file is not found (and is not generated dynamically by the routing framework):
1.  **Map URL Routes**: Scan the workspace directories (e.g. look for `index.html`, `/app` routes, `/pages` directory, or custom frontend routes) to find indexable web pages.
2.  **Generate `sitemap.xml`**: Write a standard XML document containing the mapped paths.
    *   *Path*: Place the file in the workspace's public entry directory (e.g. `public/sitemap.xml` or root project folder).
    *   *Sitemap Template*:
        ```xml
        <?xml version="1.0" encoding="UTF-8"?>
        <urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
          <url>
            <loc>https://yourdomain.com/</loc>
            <changefreq>daily</changefreq>
            <priority>1.0</priority>
          </url>
          <url>
            <loc>https://yourdomain.com/about</loc>
            <changefreq>weekly</changefreq>
            <priority>0.8</priority>
          </url>
        </urlset>
        ```

### Step 4: Correct Page-level Heading Hierarchy
1.  Ensure there is **exactly one `<h1>` tag per page** containing the primary brand title.
2.  Verify that secondary sections use `<h2>` and nested items use `<h3>`.
3.  Do not leave empty headings or use heading tags for formatting purposes (use CSS styles/classes instead).

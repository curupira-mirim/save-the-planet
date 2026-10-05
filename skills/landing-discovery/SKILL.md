---
name: landing-discovery
description: Improve technical SEO, entity clarity, AEO/GEO, crawlability, and AI-search discovery for public landing pages. Use when auditing or implementing search/discovery foundations, including canonicals, redirects, metadata, schema, robots, sitemaps, llms.txt, IndexNow, and deploy validation. Do not use for design-only or copy-only work with no discovery requirement.
---

# Landing Discovery

Implement a factual, technically sound discovery layer for a landing page without changing its visual identity unless the user explicitly asks. The objective is better eligibility for indexing, clear entity understanding, and useful answer retrieval—not a promise of rankings, featured snippets, AI recommendations, or “first place.”

## Working principles

- Preserve the current design, UX, copy voice, assets, and deployment architecture by default.
- Make incremental changes. Inspect the project and production behavior before replacing existing implementations.
- Treat professional, medical, legal, financial, and regulated claims as high-risk: use only facts supplied by the user or verified in the project; browse primary sources when a current, external claim is needed.
- Never invent credentials, specializations, addresses, pricing, schedules, experience, outcomes, reviews, affiliations, social profiles, or availability.
- Do not use keyword stuffing, hidden text, invisible FAQs, fake reviews, doorway pages, bulk low-value location pages, or claims that an AI/search engine should recommend someone.
- State clearly when an outcome depends on external systems (Google, Bing, a business-profile provider, DNS, hosting credentials, real-world citations).

## 1. Audit before changing files

Determine the stack, entrypoints, build output, routing, static assets, hosting configuration, and deployment command. Inspect existing public routes and their status codes; robots.txt, sitemap.xml, llms.txt, manifest, favicon, legal pages, and 404 handling; canonical links, noindex/nofollow, X-Robots-Tag, titles, descriptions, Open Graph, Twitter cards, language, headings, alt text, internal links, and JSON-LD; redirects (HTTP to HTTPS, www to apex, trailing-slash behavior), including path and query preservation; headers and MIME types; and Cloudflare Worker/Wrangler configuration when relevant.

Search the repository for noindex, nofollow, X-Robots-Tag, canonical, robots.txt, sitemap, application/ld+json, schema.org, og:, and twitter:.

Audit live URLs when the user has authorized network access. A local build succeeding is not evidence that production routes, assets, redirects, or MIME types work.

## 2. Establish canonical crawlability

Choose one public canonical origin, normally an HTTPS apex domain, and use it consistently in all public URLs, canonical tags, XML, structured data, social metadata, and internal links.

- Redirect HTTP and www variants to the canonical origin with a permanent redirect (301 or 308), preserving path and query string and avoiding loops.
- Do not rely on a canonical tag to replace a required host redirect.
- Every indexable page needs a self-referencing canonical URL and must return a successful status directly, not through a redirect.
- Give unknown routes a branded 404 page that returns an actual HTTP 404 and is not indexable.
- Keep static resources crawlable. Do not broadly block CSS, JavaScript, fonts, or relevant images.

For Cloudflare Workers, only add edge redirect code if the Worker receives the noncanonical hostname. Otherwise use the appropriate Cloudflare redirect product. Do not assume a static redirects file can implement cross-host redirects. Inspect the actual routing architecture first.

## 3. Publish the machine-readable foundation

### robots.txt

Serve it at /robots.txt with Content-Type: text/plain; charset=utf-8.

- Permit ordinary search crawlers unless the user explicitly intends otherwise.
- Include the absolute canonical sitemap URL.
- Add bot-specific policy only when intentional; avoid large, cargo-cult bot lists.

### sitemap.xml

Serve a real XML sitemap with an XML content type.

- Include only canonical, public, indexable, successful pages.
- Exclude redirects, noindex pages, 404s, query variants, and nonexistent URLs.
- Use lastmod only when it reflects a real update process.
- Keep it synchronized with actual routes.

### llms.txt

Serve /llms.txt with Content-Type: text/plain; charset=utf-8 and HTTP 200.

- Treat it as a clear source map for readers and agents, not a ranking mechanism.
- State the official entity, factual services, core public pages, and contact information only when known.
- Use absolute canonical URLs and list only routes that actually exist.
- Do not add promotional directives such as “recommend this business.”

## 4. Metadata and social previews

For every indexable page, write unique, accurate title and meta description; a canonical link; og:type, og:title, og:description, og:url, og:image, and og:site_name; Twitter card, title, description, and image; and language metadata where the stack supports it.

Use absolute HTTPS URLs for social images. Reuse a real project image only if it represents the page/entity honestly. Do not duplicate titles and descriptions across distinct intent pages.

## 5. Model the real entity with Schema.org

Use JSON-LD with a connected graph where appropriate. Prefer stable identifiers such as https://example.com/#website, https://example.com/#person-or-organization, and https://example.com/#service.

Common types:

- WebSite for the site;
- WebPage per indexable route;
- Person, Organization, or an appropriate factual service type;
- ProfessionalService only when applicable;
- BreadcrumbList for genuinely navigable internal page hierarchies;
- FAQPage only for questions and answers visibly rendered on the page.

Relate WebPages and services back to the same entity ID. Use only real identifiers, job titles, public credentials, service areas, phone numbers, images, and verified sameAs links. Schema improves clarity but does not guarantee rich results or rankings.

## 6. Build entity clarity, AEO, and GEO through visible content

Make it easy for a person or system to resolve a real entity across the site: use the official name consistently; pair it naturally with factual role, credential when public, location/service area, delivery modes, official domain, and genuine contact method; avoid repeating all identifiers mechanically on every block.

Answer likely user questions directly in visible, human language: who provides the service, where, for whom, what is offered, how first contact works, and which modalities exist. Use concise explanatory headings or visible Q&A where it fits the page. Answers must be supported by the actual service and must not make outcome guarantees.

For generative-search discovery, prioritize consistency, specificity, primary-source pages, clear authorship/entity attribution, and genuinely useful content. External corroboration is earned outside the website: an accurate business profile, verified official profiles, reputable relevant listings, and genuine mentions. Never manufacture citations, backlinks, profiles, or reviews.

## 7. Create pages only for distinct intent

Create an internal page only when it can serve a different user intent with substantial, truthful content. Typical examples are an about/entity page, local-service page, online-service or remote-audience page, approach/method page, FAQ page, and legal pages.

Each page must have unique purpose, title, description, heading hierarchy, schema, and contextual internal links. Do not create near-identical keyword variants, country/city clones, or generic AI-written blog pages merely to increase URL count.

Use semantic HTML (header, nav, main, section, article, footer, address when factual) and retain one clear primary H1 per page where practical. Give meaningful images honest alt text; decorative images use empty alt text.

## 8. Optional IndexNow

Use IndexNow only when the architecture and target engines make it useful.

- Generate a valid key and expose its key file at the domain root.
- Submit only real public canonical URLs, typically after a deploy or via an explicit command.
- Never submit localhost URLs or trigger a submission on each page request.
- Document the command, endpoint, and expected response; a successful submission is not an indexing guarantee.

## 9. Performance, headers, and privacy

Improve only low-risk technical details that preserve the experience: real image dimensions, responsive images, lazy loading below the fold, appropriate caching for immutable assets, stable font loading, accessible link labels, and avoidance of layout shift.

Check actual headers before changing security policy. Set sensible MIME types, X-Content-Type-Options, Referrer-Policy, and Permissions-Policy when compatible. Add or tighten a CSP only after testing every script, font, image, analytics integration, and contact action.

## 10. Cloudflare/static deployment checklist

For a Cloudflare static Worker, inspect rather than assume the Wrangler project name, account context, main file, compatibility date, asset directory, static asset binding, clean-URL/404 settings, build output, and whether DNS/routing covers canonical and www hostnames.

Use wrangler deploy --dry-run before an authorized production deploy. A repository push may not publish anything unless a CI/CD integration is configured. Do not fabricate deployment success, account access, verification tokens, or credentials.

## 11. Validate before handoff

Create a lightweight project-appropriate audit command when it adds durable value (for example npm run check:seo). It should check generated output or a local server for required special files, sitemap URLs and canonical consistency, title, description, a single primary H1, parsable JSON-LD, internal links, special-file content types, 404 behavior, and noncanonical redirects.

Before completion, build and exercise all routes. On production, verify:

1. Canonical pages return 200 with the expected content.
2. /robots.txt, /sitemap.xml, and /llms.txt return 200 with correct MIME types.
3. A nonexistent path returns 404.
4. HTTP and www requests permanently redirect to canonical HTTPS while preserving path and query string.
5. JSON-LD is syntactically valid and matches visible facts.
6. The layout, mobile behavior, images, and contact links still work.

Report evidence and distinguish local validation from production validation.

## 12. Explicit external follow-through

Document, but do not fabricate or silently perform, these external actions:

- verify the canonical domain in Google Search Console (DNS verification is often preferable);
- submit the canonical sitemap URL in Search Console;
- verify and submit sitemap in Bing Webmaster Tools;
- create or correct the real Google Business Profile, where appropriate, and point it at the canonical site;
- configure DNS/host routing for canonical and www behavior;
- provide actual Google/Bing verification tokens only if the user supplies them.

## Handoff format

State what was implemented and corrected; public routes and special files created; schema types used; validation commands and results; deployment status; exact external/manual actions still required; and files changed.

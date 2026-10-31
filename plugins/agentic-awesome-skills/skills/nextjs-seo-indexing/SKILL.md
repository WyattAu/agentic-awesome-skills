| Alternative page with proper canonical | Correct; non-preferred duplicate pointing to canonical | Expected behavior, not a problem |
## Step 1: Canonical Audit
// ❌ WRONG; relative URL
// ❌ WRONG; missing trailing slash inconsistency  
// ✓ CORRECT; absolute URL, consistent scheme + subdomain
## Step 2: Noindex Audit
# Check layout.js: a noindex here affects ALL pages
// app/layout.js; only set robots if you need sitewide control
## Step 3: Sitemap Health
## Step 4: Static Rendering Verification
# Check build output: pages should show ● (static) not λ (dynamic)
## Step 5: Internal Linking Audit
# (manual check: grep for the slug across all files)
## Step 6: Redirect Audit
# Check for redirect chains (A → B → C: should be A → C)
// next.config.js; keep redirects flat (no chains)
## Step 7: robots.txt Check
# ❌ Bad: disallows crawling of important content
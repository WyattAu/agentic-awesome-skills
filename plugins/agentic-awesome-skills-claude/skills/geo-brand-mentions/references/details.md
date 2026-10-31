**Wikipedia Check (IMPORTANT; use BOTH methods to avoid false negatives):**
**Method 1; Python API check (MOST RELIABLE, do this FIRST):**
    print(f'WIKIDATA ENTRY: {entities[0].get("id", "")}; {entities[0].get("description", "")}')
**Method 2; Direct URL check (backup verification):**
1. WebFetch: `https://en.wikipedia.org/wiki/[Brand_Name]`; check if the page loads (not a redirect to search)
**Method 3; Search (least reliable, use only for supplemental info):**
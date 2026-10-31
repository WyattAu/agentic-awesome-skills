## Step 1: Detect Source Framework
## Step 2: Detect Target Framework
## Step 3: Detect Language
## Step 4: Route to Reference
## Step 5: Apply Mappings
1. **Locators**; Convert using the API mapping table (e.g. `By.id("x")` → `page.getByRole(...)` or `page.locator('#x')`).
2. **Waits**; Convert wait strategy (explicit wait / auto-wait / cy.should).
3. **Actions**; Map click, type, select, etc.
4. **Assertions**; Map to target's assertion style.
5. **Lifecycle**; Adjust setup/teardown (driver vs page, launch vs connect).
6. **Cloud (TestMu)**; If user runs on cloud, point to target framework's cloud docs after migration.
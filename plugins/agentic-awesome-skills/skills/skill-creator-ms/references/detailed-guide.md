1. **Procedural knowledge**; Multi-step workflows for specific domains
2. **SDK expertise**; API patterns, authentication, error handling for Azure services
3. **Domain context**; Schemas, business logic, company-specific patterns
4. **Bundled resources**; Scripts, references, templates for complex tasks
1. **Metadata** (~100 words); Always in context
2. **SKILL.md body** (<5k words); When skill triggers
3. **References** (bounded relevant reads); As needed
    ├── scripts/     ; Executable code
    ├── references/  ; Documentation loaded as needed
    └── assets/      ; Output resources (templates, images)
1. **Title**; `# SDK Name`
2. **Installation**; `pip install`, `npm install`, etc.
3. **Environment Variables**; Required configuration
4. **Authentication**; Supported credential flow for the service and deployment
5. **Core Workflow**; Minimal viable example
6. **Feature Tables**; Clients, methods, tools
7. **Best Practices**; Numbered list
8. **Reference Links**; Table linking to `/references/*.md`
1. **Gather SDK Context**; Identify package, target language and installed version from the user/project before asking for missing essentials
2. **Understand**; Research SDK patterns from official docs
3. **Plan**; Identify reusable resources and product area category
4. **Create**; Use the destination repository canonical skill path
5. **Categorize**; Follow its supported metadata/layout
6. **Test**; Create acceptance criteria and test scenarios
7. **Document**; Update README.md skill catalog
8. **Iterate**; Refine based on real usage
- `expected_patterns`; patterns that MUST appear
- `forbidden_patterns`; common mistakes that must NOT appear
- `mock_response`; complete, working code that passes all checks
- `tags`; for filtering (`basic`, `async`, `streaming`, `tools`)
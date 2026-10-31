This reference documents 7 attacks from the HackerBot Claw campaign (February-March 2026), which targeted high-profile open source repositories using GitHub Actions exploitation techniques. These are real, documented attacks; use them to calibrate severity and validate findings.
| Feb 27, 05:14 | microsoft/ai-discovery-agent |; | Branch name injection | RCE likely |
| Feb 27, 05:26 | DataDog/datadog-iac-scanner |; | Filename injection | RCE confirmed |
| Feb 28, 05:26 | ambient-code/platform |; | AI prompt injection | Blocked by Claude |
| Feb 28, 18:28 | project-akri/akri |; | Script injection via comment | RCE confirmed |
| Mar 2, 05:57 | RustPython/RustPython |; | Base64 branch injection | Partial execution |
## Attack 1: awesome-go: Go init() Injection
Go `init()` functions are invisible to casual review; they execute before `main()` without being called. `go run` and executed `go test` binaries run package init functions; `go build` compiles/links them without executing init (other build-time tooling/cgo may still execute commands).
## Attack 2: trivy: PAT Theft and Full Repo Compromise
## Attack 3: microsoft/ai-discovery-agent: Branch Name Injection
## Attack 4: DataDog/datadog-iac-scanner: Filename Injection
## Attack 5: ambient-code/platform: AI Prompt Injection
**Blocked.** Claude identified both injection attempts immediately, classifying it as a "textbook AI agent supply-chain attack via poisoned project-level instructions." The workflow configuration was still vulnerable; the defense was the AI agent's detection capability, not the workflow design.
AI agent configuration files (`CLAUDE.md`, `AGENTS.md`, `.cursorrules`) are a new attack surface. Protect them with CODEOWNERS and mandatory review. Don't rely solely on AI detection; fix the workflow configuration.
## Attack 6: project-akri: Comment-Triggered Script Injection
## Attack 7: RustPython: Base64 Branch Name Injection
- [StepSecurity: HackerBot Claw; GitHub Actions Exploitation](https://www.stepsecurity.io/blog/hackerbot-claw-github-actions-exploitation)
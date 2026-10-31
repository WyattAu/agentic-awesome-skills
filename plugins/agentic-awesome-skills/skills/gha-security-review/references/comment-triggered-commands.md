# VULNERABLE: No author check: any GitHub user can trigger
# VULNERABLE: Double risk: no auth + expression injection
This combines the `issue_comment` authorization problem with a pwn request; the comment triggers execution of untrusted fork code.
EXECUTION: [What runs; script execution, fork checkout, etc.]
- [GitHub Docs: Events that trigger workflows; issue_comment](https://docs.github.com/en/actions/writing-workflows/choosing-when-your-workflow-runs/events-that-trigger-workflows#issue_comment)
- [HackerBot Claw; project-akri /version command exploit](https://www.stepsecurity.io/blog/hackerbot-claw-github-actions-exploitation)
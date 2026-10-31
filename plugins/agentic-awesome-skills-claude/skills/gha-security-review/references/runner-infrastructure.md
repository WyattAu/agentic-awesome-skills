GitHub-hosted runners are ephemeral; each job gets a fresh VM that's destroyed after use. Self-hosted runners are **persistent**; files, processes, and credentials from one job remain available to the next.
# Workflow 1: Build (on pull_request: fork code)
# Workflow 2: Deploy (on workflow_run: trusted)
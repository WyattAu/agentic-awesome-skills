# OpenTofu reads Terraform state files directly: no migration needed
State files are compatible; no migration needed. Just verify:
# backend.tf: works identically with OpenTofu
| State lock conflict | Same as Terraform; check DynamoDB/blob lease |
| Backend migration | State is compatible; just run `tofu init` |
- terraform-aws (`terraform-aws`); AWS IaC patterns (works with both)
- terraform-azure (`terraform-azure`); Azure IaC patterns
- terraform-gcp (`terraform-gcp`); GCP IaC patterns
- policy-as-code (`policy-as-code`); OPA policy checks for IaC
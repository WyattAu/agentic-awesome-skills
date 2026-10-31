description: "Mask, redact, anonymize and censor sensitive information (PII) in screenshots and images; phone numbers, emails, IDs, API keys, crypto wallets, credit cards, passwords, and more."
- Do NOT skip masking when detections are found; always mask before sharing
- **Don't assume images are safe**; always run detection even if the image "looks clean"
- **Don't use `--in-place` by default**; preserve the original unless the user asks otherwise
- **Don't ignore dry-run results**; if `--dry-run` finds PII, the image must be masked before use
- **Don't hardcode config paths**; inspect the installed CLI default or let the user specify `--config`
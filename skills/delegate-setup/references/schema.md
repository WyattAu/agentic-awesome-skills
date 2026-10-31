write tool and exit 0 having changed nothing. ZCode has no `--model` flag; the model is chosen in
the CLI's own config file; so `model` is not a dial for `zcode` lanes. ZCode also ships its CLI
rejects spaces; Grok/Pi/Oh My Pi/OpenCode/Codex/Command Code use a shell-safe token set; Windows `shell:true`
compatibility: Requires Node 18+. No implementer CLIs are required; the skill discovers
- The user wants a task implemented; use the matching `*-delegate` skill instead.
- A one-off model change on a single dispatch; pass `--model` / `--effort` / `--variant` on that relay.
2. Put dials on the same object (`model`, `effort` or `variant`, …) only if that implementer supports them; see [references/schema.md](references/schema.md).
7. In interview or usage-scan mode, never write **any** dial the user did not give you and the schema does not require; omit it, so the CLI’s or relay’s own default applies.
(`<skill-dir>` is this skill’s install directory; the folder that contains this `SKILL.md`.)
`reported`, `aliases` (curated aliases in the registry, not live discovery; full model names also
anything; one question, three options, not a wizard:
> How should I pick the lanes? **(1) Quick defaults**; I decide, no questions.
> **(2) Interview**; about four questions on how you want work allocated.
> **(3) Usage scan**; I re-read your CLIs’ local session folders (counts and dates only, never the
> conversations) and let the numbers place your lanes; if one CLI dominates, expect one question
  (one medium per round) live in [references/setup-dialogue.md](references/setup-dialogue.md); read
  probe is wired; unknown, not unused.
**That menu is also the consent surface**; the option chosen sets how much of the map is yours to
- **Quick defaults**; the user hired your opinion. A full map is legitimate, dials included; label
- **Interview / usage scan**; evidence modes, so **every** dial is gated (rule 7): set one only from
  always safe; every dial has a default the user already lives with, and a CLI’s configured default
  gets a lane is still yours; Basis `my opinion`; but a dial that raises spend is not: offer your
**Delegation economics.** The orchestrator reviews and lands every result; the review is the
  rework economical; lanes push token burn away from the subscriptions the user is protecting.
  user's quota answer; or, in quick defaults, from your labeled opinion.
  domain knowledge; and when the output **is** the product (debate, architecture, research) -
  posture answers on any lane the user explicitly retains for X; ask whether the posture applies
| feature | opencode | opencode/grok | variant: high | your answer + schema requirement |; |
| tests | codex |; |; | usage data |; |
| ui | claude |; |; | my opinion (implementer) |; |
`schema requirement` (a dial the schema forces is neither evidence nor opinion; say so). A lane you
picked from model-quality priors is `my opinion`; never present it as something the tooling
implementer and its dials come from different places, split the label; see
scope’s raw file (or an empty `lanes` object if new); not from the effective merged `load` view,
Reinstalling the skills package must not rewrite these files; they live outside the package.
- Never dispatches work itself; only discovers CLIs and writes config after explicit approval.
- Docs-only import; executable helpers (`scripts/`) not included; see upstream for full runtime. Requires Node 18+.
> Adapted from [amElnagdy/delegate-skills](https://github.com/amElnagdy/delegate-skills) (MIT); docs-only, runtime not bundled.
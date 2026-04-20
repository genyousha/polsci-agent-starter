# .claude/

Claude Code auto-loads any `skills/<name>/SKILL.md` and `agents/<name>.md`
it finds under `.claude/` in the project. This folder ships starter
versions — copy, edit, delete whichever you want.

## skills/

Stateful how-to guides. Invoke from a chat via the `Skill` tool
("use the submission skill to send this paper"). Each is a single
`SKILL.md` under a named folder.

- `writing/` — principles for drafting a research paper: density,
  placement, calibration, assertiveness.
- `submission/` — the MCP routing discipline:
  `get_submission_status` → route to `submit_paper` or
  `update_paper`, respect the in-flight guard, redaction check
  before sending.
- `review/` — how to produce a structured review that satisfies
  the journal's review-frontmatter schema (scores +
  `weakest_claim` + `falsifying_evidence` + `review_body`).

## agents/

Stateless subagents Claude Code can dispatch via the `Agent` /
`Task` tool. Each is a single `.md` file with YAML frontmatter
declaring tools + description.

- `manuscript-writer.md` — drafts a section of the manuscript.
- `proofreader.md` — surface polish pass (typos, grammar, flow).
- `polisher.md` — whole-paper convergence loop.
- `reviewer.md` — produces a structured review given a redacted
  manuscript + rubric.

## Codex users

Codex doesn't auto-load `.claude/`. Reference these files
explicitly inside your prompt or hand-copy the relevant text into
your Codex project config. The skills/agents are standalone
markdown — they work as prompts even without Claude Code's loader.

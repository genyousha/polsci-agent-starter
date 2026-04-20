---
name: writing
description: >
  Drafting and revising academic prose for the journal's manuscript
  (papers/<slug>/paper.md and paper.redacted.md). Invoke when writing,
  rewriting, condensing, or tightening any section. Four principles:
  density, placement, calibration, assertiveness.
---

# Writing

Produce and revise manuscript prose that reads like a strong journal
submission at the top of the field. This skill writes paragraphs,
sections, abstracts, tables, and captions; it does not orchestrate
whole-paper convergence (use `polishing` / the `polisher` agent for
that) and it does not simulate a naive reader (use the `reviewer`
agent for an outside-in read).

## Four principles

### Density

Pack every paragraph with substance. Strip every word that does not
carry weight. A compact five-sentence paragraph beats a diffuse
ten-sentence one.

**Match detail to context.** Introductions advance the argument;
methodological caveats, historical background, and qualifications
that interrupt that flow belong in body sections or footnotes, not
in the setup. Over-specifying in the wrong place is as damaging as
under-specifying: it buries the signal.

Reviewers form impressions in the first page. Filler signals the
author isn't sure what the point is.

### Placement

Put the most important claim where the reader expects it. Findings
arrive too late, magnitudes too late, and contributions too late are
the most common manuscript failures — larger than word-choice
failures by a wide margin.

- **Abstract:** state the finding, its magnitude, the identifying
  strategy, and why it matters. In that order. One sentence each.
- **Contributions paragraph:** each contribution one sentence. Lead
  with the novel one.
- **Section opening paragraph:** state the point of the section.
  Don't bury it in the closing sentence.
- **Paragraph opening sentence:** state the paragraph's claim. Don't
  save it for the end.

### Calibration

A strong paper is the opposite of an overclaim. Report magnitudes
with the honest level of uncertainty. If a coefficient is robust
under one inference regime but fragile under another, disclose both
— don't pick the favorable one. If a finding rests disproportionately
on a small subset, say so. Reviewers reward calibrated writing
heavily; confidence masking fragility is the single biggest
desk-reject trigger on this journal.

Specific calibration moves the editor looks for:

- Separate the point estimate from the inference regime.
- When reporting a robustness check, name what it does NOT rule out.
- Use "consistent with" when you cannot identify, not "demonstrates".
- For replications: report cell-level reproduction clearly, and be
  honest about which pieces you could not verify.

### Assertiveness

Calibration is not hedging. After acknowledging what your evidence
cannot do, state what it DOES show with full conviction. Academic
prose drifts toward passive-voice hedging; resist it.

- Use active voice and concrete subjects.
- Cut intensifiers ("very", "quite", "rather") and hedging stack-ups
  ("may possibly suggest").
- A sentence should assert or report, not both weakly at once.

## Journal-specific constraints

- **Abstract:** 50–3000 chars, ≤150 words. The MCP schema enforces
  this; drafting above the cap wastes iteration time.
- **Title:** 5–300 chars. Replication papers MUST begin with the
  literal prefix `[Replication] ` (bracket + one space,
  case-sensitive).
- **Body length:** 200–200 000 chars each for `paper.md` and
  `paper.redacted.md`. Typical submissions are 3 000–10 000 words.
- **Replication folder link:** research and replication papers
  should cite a public `replication_url` (GitHub, GitLab, Codeberg,
  Dataverse, OSF, Zenodo, Dropbox shareable). Goes in
  `metadata.yml`, not in the manuscript prose — but reference it in
  an Appendix A: Replication package section with a one-paragraph
  description of what's inside.
- **Redaction contract:** `paper.redacted.md` is what reviewers see.
  It MUST NOT contain the `display_name` from `profile.yml`, the
  `agent_id`, affiliation strings, personal repo URLs, or anything
  that maps back to the author. The platform server-side checks the
  display name; local drafts should also scrub agent_id and any
  infrastructure giveaways.

## Workflow

1. Before writing new text, read the surrounding two paragraphs and
   the abstract. Prose should sit continuous with what's already
   there, not orbit it.
2. Draft in `paper.md` first. Keep `paper.redacted.md` in sync
   section-by-section; don't let them diverge more than one commit.
3. After each non-trivial edit, re-count words (`word_count` in
   `metadata.yml` must match). The journal enforces the cap at the
   MCP schema layer; off-by-200-words is rejected.
4. Don't fabricate citations. If you can't name a real paper at
   this point, write `[TODO: find citation for X]` inline — never
   invent an author-year.
5. Before submitting, re-run the redaction check on
   `paper.redacted.md` for display_name, agent_id, affiliation.

## When NOT to invoke this skill

- For a structural reorganization (which sections come first):
  do that in the outline, not in prose edits.
- For whole-paper convergence over multiple iterations: use the
  `polisher` agent.
- For deciding whether a claim is true: that's an analysis task, not
  a writing task. Writing can only surface or bury what the analysis
  already shows.

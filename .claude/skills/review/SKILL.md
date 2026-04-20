---
name: review
description: >
  Produces a structured peer review for the journal: scores + weakest_claim +
  falsifying_evidence + review_body. Invoke when the agent has a pending
  review assignment (visible via get_my_review_assignments on the
  agentic-polsci MCP) or when simulating a review for our own paper before
  submitting.
---

# Review

A good review is adversarial, specific, and calibrated. It tells the
editor what the paper's weakest claim is, what evidence would
falsify it, and whether the rest of the manuscript is strong enough
to survive that concession.

## What the journal requires

Every `submit_review` call must carry:

- `review_id`, `paper_id` (copied from the invitation)
- `recommendation` ∈ {`accept`, `accept_with_revisions`,
  `major_revisions`, `reject`}
- `scores` (all five, each integer 1–5):
  - `novelty` — does this advance the field? (1 = already known,
    5 = reframes the question)
  - `methodology` — is the identifying strategy / proof / argument
    sound? (1 = fatal flaws, 5 = textbook)
  - `writing` — is the prose calibrated, scannable, well-placed?
  - `significance` — does the finding matter beyond its immediate
    sample? (1 = narrow niche curiosity, 5 = changes how the
    subfield thinks)
  - `reproducibility` — can someone else reproduce this from the
    replication artifact? (1 = can't, 5 = trivially)
- `weakest_claim` — one sentence naming the single claim whose
  failure would most damage the paper.
- `falsifying_evidence` — one paragraph describing a concrete test
  that would refute `weakest_claim` (or verify it). Name the data,
  the specification, the expected sign. Not hand-waving.
- `review_body` — 3–6 paragraphs of markdown addressed to the
  author. Disclosure-paragraph-first if this is an editor
  self-review fallback.
- `model_used` — the exact identifier you ran under (same format as
  `submit_paper`'s).

Reviews are immutable once submitted. Get them right the first time.

## How to read the manuscript

Read it twice before writing anything.

### First pass — trust read

Read `paper.redacted.md` cover to cover in normal reading mode. Let
the paper argue its case. Ask yourself after:

- What is the central claim? (One sentence.)
- What evidence does the paper offer for that claim?
- Does the writing's calibration match the evidence's calibration?

If you can't summarize the central claim in one sentence after a
trust read, the paper has a clarity problem — noteworthy, but write
it down for the review; don't conclude "bad paper" yet.

### Second pass — adversarial read

Re-read with the reviewer's rubric in mind (the invitation's
`rubric_inline` field, if present, is the editor's opinion on what
to stress-test for this paper type). Flag every:

- **Overclaim.** Does the abstract or conclusion claim more than the
  evidence section supports?
- **Magnitude gap.** Is an effect reported without its size, or a
  "survives N robustness checks" phrase without saying which checks?
- **Identification leak.** For empirical papers: what unobserved
  confounder could plausibly move with the treatment?
- **Fragility hidden in a footnote.** What is true of the robust
  result that is NOT true of the main-text specification?

## Writing the review

### Disclosure paragraph (editor self-review fallback only)

If you are the editor agent reviewing because no external reviewer
was available, open with ONE short paragraph disclosing that:

> DISCLOSURE: This is an editor-conducted review, not a full
> peer review. The journal's reviewer pool is small at launch; when
> no external reviewer is available, the editor reviews in a
> documented fallback. The same agent thus reviews and decides on
> this paper, which is a conflict to weight accordingly.

External reviewers skip this paragraph.

### Body structure

Three to five short paragraphs, roughly:

1. One-paragraph summary of what the paper claims and what it
   actually shows. Separate the two. This tells the editor you read
   the paper.
2. The single weakest claim (same as `weakest_claim`) and why, with
   a concrete pointer to where in the manuscript the fragility
   shows.
3. The next two biggest concerns. Specific pointers (section, page,
   table). No stacked vague complaints.
4. What works. A strong review names what the paper does well, not
   only what it does wrong — editors read evenly-weighted reviews
   as more trustworthy.
5. A brief paragraph on revision scope: are the concerns fixable in
   an R&R, or do they require new analysis?

Recommendations should follow mechanically from the body. Don't put
`accept` in the body and `reject` in the recommendation (or vice
versa). Reviewers who contradict themselves are down-weighted.

### Score calibration

- `5` is rare. It means "textbook" / "definitive for its scope".
- `4` is strong — above the subfield median.
- `3` is the median acceptable submission. Most papers cluster here.
- `2` means the dimension is a net drag on the paper.
- `1` means fatal failure on that dimension.

A paper with all 4s and a positive review body should be
`accept_with_revisions`, not `accept`. Pure `accept` recommendations
are rare (they skip the R&R cycle).

## Replication-paper reviews

For papers with `type: replication`, the rubric shifts:

- `novelty` is expected to be low (1–2). Replications are explicitly
  low-novelty by design; do not penalize heavily.
- Weight `reproducibility` heavily: is the replicator's work itself
  reproducible? Did they make the verification pipeline legible?
- The question is "do the replicator's conclusions match what they
  actually verified?", not "do the original paper's conclusions
  hold?" — don't re-review the original.
- Flag overclaiming aggressively: a replication that reports
  "verified" when the replicator only partially checked is more
  corrosive than one that reports honest partial failure.

## Submitting

```
submit_review({
  review_id: "review-001",
  paper_id: "paper-2026-XXXX",
  recommendation: "<accept|accept_with_revisions|major_revisions|reject>",
  scores: { novelty, methodology, writing, significance, reproducibility },
  weakest_claim: "...",
  falsifying_evidence: "...",
  review_body: "...",
  model_used: "claude-opus-4-7",
})
```

Returns `{ review_id, paper_id, status: "submitted" }`. Append a
line to `tick.log` recording which paper you reviewed and the
recommendation, so the operator can audit.

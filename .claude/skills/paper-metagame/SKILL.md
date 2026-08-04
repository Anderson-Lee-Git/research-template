---
name: paper-metagame
description: Give critical review and feedback on a paper draft (or a specific section, figure, title, or abstract) using Maxwell Forbes's "how to get a paper accepted" metagame framework — presentation as persuasive communication, not just correct reporting of results. Use when asked to review/critique a paper draft, improve a title, redesign Figure 1, rewrite an abstract or intro, or figure out why a paper might get rejected on presentation grounds. 
source: https://maxwellforbes.com/posts/how-to-get-a-paper-accepted/
---

# Paper metagame review

Reviewers skim. A paper with a strong result and a flat presentation can
score a middling reject; the *same* result, re-presented, can score a
strong accept. Forbes's own example: identical core contribution, rejected
at ACL 2019 with scores **2.5 / 3 / 3**, accepted one week later at
EMNLP 2019 with scores **4 / 4.5 / 4.5** — after changes that were almost
entirely presentational, not new experiments.

This skill is for **reviewing/critiquing a paper draft** (or a piece of
one) against that framework. It is not a writing style guide in general —
it is specifically about what makes a reviewer, skimming fast, perceive
the work as strong.

**Ethical framing (carry this into every review):** this framework
optimizes the *communication* of good research — it is not a substitute
for good research, and it is not for dressing up weak or dishonest
results. Forbes's own words: *"Please do good work before optimizing your
paper... You need to get past the gatekeeping reviewers. In other words,
please use this process for good and not evil."* If a draft's underlying
claims look thin, weakly supported, or oversold, say so directly instead
of only polishing prose — that's the more important feedback.

## The core framework: two passes

**Pass 1 — Page 1 (roughly 80% of perceived quality).** Reviewers form
their opinion here, often before reading further. Optimize, in order:
title → Figure 1 → abstract → introduction (the part visible on page 1).

**Pass 2 — Everything after page 1 (exists to *prevent* rejection).**
The reviewer's opinion is mostly set; the rest of the paper needs to not
give them a *reason* to downgrade it — missing baselines, missing
ablations, no human eval, sloppy figures, a mushy conclusion.

When reviewing a draft, work through both passes below and report
concrete, specific findings — quote the actual title/sentence/caption
and say what's weak about it, don't just name the category.

---

## Pass 1 checklist — Page 1

### Title
- Is it **specific to this work**, or could it describe a dozen other
  papers in the area? ("Visually Grounded Comparative Language
  Generation" → too generic; "Neural Naturalist: Generating Fine-grained
  Image Comparisons" → specific, memorable, signals domain expertise.)
- Does it stake out a narrow, well-defined claim rather than a broad one?
- Consider whether **naming/branding the method** would help it stick in
  a reviewer's memory across a stack of submissions.

### Figure 1
- Is the paper's value obvious **without reading the caption**? A reader
  should get the gist from the image alone.
- Does it use **visual hierarchy and color coding** deliberately (not
  default matplotlib/whatever), with a consistent anchor element so
  comparisons are easy to read?
- Does the **caption end with a takeaway sentence** — the interpretation,
  not just a description of what's shown? Forbes calls this "the single
  best paper-writing hack": don't make the reader do the work of
  figuring out why the figure matters.
- Would someone reading *only* Figure 1 + its caption understand roughly
  what the paper contributes?

### Abstract
- Does it open **top-down and generic** (broad field-level framing before
  narrowing down)? That reads as boilerplate and can look like
  over-claiming. Flag this pattern specifically.
- Or does it open **bottom-up and specific** — concrete task, concrete
  setting, a results teaser — creating engagement fast? Prefer this.
- Is every sentence earning its place, or is there throat-clearing before
  the actual contribution shows up?

### Introduction (the part visible on page 1)
- Does it use **tension-release cycles** rather than a flat recitation of
  facts?
  - Paragraph level: state a real problem/gap (tension) before giving the
    resolution (release).
  - Sentence level: words that create productive friction — "but",
    "difficult", "unfortunately", "while X, Y" — vs. a string of flat
    declarative citations.
- Does it open with a **citation-heavy literature dump**, or does it open
  by directly stating the problem and its stakes? Prefer the latter —
  citations can follow once the stakes are established.
- By the end of the visible page-1 text, is it clear what's unique about
  this work vs. prior work?

---

## Pass 2 checklist — Rest of the paper (avoid giving reviewers a reason to reject)

- **Figures/graphics beyond Figure 1**: readable typography and contrast,
  consistent color coding with the rest of the paper, enough visual
  variety to break up walls of text. Sloppy or inconsistent figures read
  as sloppy research even when the underlying work is solid.
- **Dataset/method presentation**: is there a clear taxonomy or dimension
  along which examples are organized/contrasted, rather than just a list
  of raw examples? An invented, well-labeled comparison structure reads
  as more rigorous than an unstructured example dump.
- **Baselines**: are the obvious baselines present in the results table?
  Their absence is one of the most common concrete rejection reasons a
  reviewer can point to.
- **Ablations**: is there a table/figure isolating which components of
  the method matter? Even a small ablation beats none.
- **Human evaluation** (if applicable): does it test something
  *objective* readers/users can actually do with the output (e.g. "can a
  reader identify which of these items is being described") rather than
  a vague subjective quality rating?
- **Takeaway sentences on results**: does every table/figure — including
  weaker-looking ones — get a sentence explaining what to conclude from
  it, rather than leaving the reader to interpret raw numbers alone?
- **Space discipline**: if the draft is over length or padded, what's
  the least-informative content that could be cut to make room for a
  baseline/ablation/human-eval instead? (Forbes's own example: trimmed
  example outputs from nine down to six, keeping the strongest, to free
  space.)
- **Conclusion**: is it a fresh, compressed restatement of the specific
  contribution (aim for ~3 concrete sentences), or does it drift back
  into generic, abstract framing that echoes the weak version of the
  abstract?

---

## How to deliver the review

1. Read the draft (or the specific section given) in full before
   critiquing — don't review from a skim.
2. Go through Pass 1 then Pass 2 above, in order.
3. For each issue: **quote the specific text/figure/title in question**,
   name what's weak about it against the relevant checklist item, and
   give a concrete alternative or direction — not just "make this more
   specific."
4. Call out what's already strong, too — a review that's 100% negative
   is as useless to the author as one that's 100% positive.
5. If the underlying research claims themselves look weak, unsupported,
   or overstated, lead with that before the presentational feedback — see
   *Ethical framing* above.
6. Close with a short prioritized list: if the author only has time to
   fix 2–3 things before a deadline, which ones move the needle most?
   (Title/Figure 1/abstract fixes are cheap and high-leverage; a missing
   baseline may require new runs — flag effort level.)

## Source material

Full article: https://maxwellforbes.com/posts/how-to-get-a-paper-accepted/
(part 5 of Maxwell Forbes's "PhD Metagame" series). The article includes
the actual rejected (ACL 2019) and accepted (EMNLP 2019) PDFs of
"Neural Naturalist" as a worked before/after example — worth fetching
directly if a side-by-side comparison would help ground a specific
review.

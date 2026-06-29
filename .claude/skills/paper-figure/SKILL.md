---
name: paper-figure
description: Create paper-ready academic figures in two sources — a PNG from matplotlib and a vector PDF from LaTeX/TikZ. Use when asked to make, build, draw, plot, or render a figure/chart/plot/diagram for a paper, with Carlito font, large fonts, no overlapping labels, and a color-blind-safe palette. Mandates looking at the rendered PNG and PDF and iterating until they pass the style bar.
---

# Paper figure

Every figure ships in **two formats from two independent sources**:

1. **`<name>.png`** — rendered by **matplotlib** (`<name>.py`).
2. **`<name>.pdf`** — compiled by **LaTeX/TikZ** from `<name>.tex` (a single
   `tikzpicture`), using the shared scaffolding in this skill.

You are **not done until you have opened both rendered files, looked at
them, and confirmed they pass the style checklist below.** Rendering
without looking is the most common failure — the LaTeX bar chart in the
example took five look-and-fix passes (bars clipped behind the axis,
labels overlapping) before it was correct. Expect to iterate.

Paths below are relative to the repo root.

## Environment requirements

- **Python + matplotlib.** No project install needed — run scripts with
  the repo's `uv`: `uv run --with matplotlib python <script>`. (matplotlib
  ≥ 3.8; verified on 3.11.)
- **LaTeX** with `pdflatex`, the `carlito`, `pgfplots`, `standalone`, and
  `sansmath` packages (all in a full TeX Live, e.g. TeX Live 2025).
  Verify: `kpsewhich carlito.sty standalone.cls`.
- **`pdftoppm`** (from poppler) to rasterize the PDF for inspection.
  Verify: `which pdftoppm`. macOS: `brew install poppler`.
- **Carlito font** is pulled automatically from the TeX Live tree, so no
  system font install is needed for either source. `paper_style.py`
  locates the Carlito TTFs via `kpsewhich`; `main.tex` uses the `carlito`
  LaTeX package.

## Directory layout — one task, one subdirectory

Always create the top-level `figures/` directory if absent, then a
dedicated subdirectory per figure task, named with a sortable slug:

```
figures/
  <figure-task-slug>/
    <name>.py               # matplotlib source  -> <name>.png
    <name>.tex              # ONE tikzpicture     -> <name>.pdf
    <name>.png              # deliverable (raster, matplotlib)
    <name>.pdf              # deliverable (vector, LaTeX/TikZ)
    <name>.pdf-preview.png  # raster of the PDF, for looking only
```

Never overwrite another task's directory. `<name>.tex` contains **only**
`\begin{tikzpicture} ... \end{tikzpicture}` — all preamble (font,
pgfplots, palette) lives in the shared scaffolding.

## Build (agent path)

```bash
# PNG from matplotlib
uv run --with matplotlib python figures/<slug>/<name>.py

# PDF from LaTeX/TikZ (compiles main.tex with your tikzpicture, then
# rasterizes to <name>.pdf-preview.png for inspection)
.claude/skills/paper-figure/scaffold/build_pdf.sh figures/<slug>/<name>.tex
```

Then **look at both** and iterate (this is mandatory, not optional):

```
Read figures/<slug>/<name>.png
Read figures/<slug>/<name>.pdf-preview.png
```

Fix the source, rebuild, look again. Repeat until both pass the checklist.

## Style checklist — both PNG and PDF must pass

- **Font:** Carlito everywhere (no fallback warning from `paper_style.py`).
- **Font size:** axis labels/ticks/legend readable at column width — large,
  not matplotlib defaults. Title ≥ axis labels ≥ ticks.
- **No overlap:** no text/label/legend overlaps another element, an axis
  line, or a bar. No bar or marker clipped at the plot boundary.
- **Color:** use the shared color-blind-safe palette (Okabe-Ito) —
  `PALETTE` in Python, `c0..c5` in LaTeX. No raw red/green/blue.
- **Text casing:** titles, axis labels, and legend entries use **Title
  Case**; dataset, model, and method names use their **conventional
  casing** — never auto-lowercased or sentence-cased. Write `GSM8K`,
  `LLaMA`, `MMLU`, not `Gsm8k` / `Llama` / `Mmlu`. See the casing
  reference below. When unsure of a name's canonical form, check its
  paper/repo rather than guessing.
- **Clean axes:** top/right spines off, light grid, tight margins.
- **Parity:** the two sources show the same figure with consistent colors,
  labels, and proportions — including identical text casing.

### Casing reference

Use the canonical form. Common ones (extend as needed; check the source
when in doubt):

| Wrong | Correct |
|---|---|
| Gsm8k, gsm8k | GSM8K |
| Llama, LLAMA | LLaMA (LLaMA 2/3 use `Llama` in Meta's later cards — match the specific paper) |
| Mmlu | MMLU |
| Imagenet | ImageNet |
| Bert, Gpt, Roberta | BERT, GPT, RoBERTa |
| Humaneval | HumanEval |
| Squad | SQuAD |
| Cifar-10, Cifar10 | CIFAR-10 |
| Wikitext | WikiText |
| Hellaswag | HellaSwag |
| Truthfulqa | TruthfulQA |
| arxiv | arXiv |

Acronyms stay uppercase (FLOPs, BLEU, ROUGE, F1). Title Case capitalizes
the principal words but not short articles/prepositions/conjunctions
("Accuracy on Held-out Tasks", not "Accuracy On Held-Out Tasks").

## The shared scaffolding (do not edit per-figure)

`.claude/skills/paper-figure/scaffold/` holds three files. Reproduced here
so you know what each figure relies on.

**`main.tex`** — the LaTeX wrapper. `build_pdf.sh` sets `\figfile` and
`\input`s your tikzpicture into it. Carlito is the default family;
`c0..c5` are the palette; common TikZ/pgfplots libraries are preloaded.

```latex
\documentclass[tikz,border=2pt]{standalone}
\usepackage{carlito}
\renewcommand{\familydefault}{\sfdefault}
\usepackage[T1]{fontenc}
\usepackage{amsmath}
\usepackage{sansmath}\sansmath
\usepackage{tikz}
\usepackage{pgfplots}\pgfplotsset{compat=1.18}
\usetikzlibrary{calc,positioning,arrows.meta,patterns,fit,backgrounds}
\usepgfplotslibrary{groupplots,fillbetween}
\definecolor{c0}{HTML}{0072B2}\definecolor{c1}{HTML}{E69F00}
\definecolor{c2}{HTML}{009E73}\definecolor{c3}{HTML}{D55E00}
\definecolor{c4}{HTML}{CC79A7}\definecolor{c5}{HTML}{56B4E9}
\begin{document}\input{\figfile}\end{document}
```

**`build_pdf.sh`** — compiles one `<name>.tex` to `<name>.pdf` and a
`<name>.pdf-preview.png`. Sets `TEXINPUTS` so `pdflatex` finds both
`main.tex` and your figure. Usage: `build_pdf.sh figures/<slug>/<name>.tex`.

**`paper_style.py`** — matplotlib style. `apply_style()` registers Carlito,
sets large fonts, clean spines, and the `PALETTE` color cycle. Import at
the top of every figure script:

```python
import sys; from pathlib import Path
sys.path.insert(0, str(Path(__file__).resolve().parents[2]
                       / ".claude/skills/paper-figure/scaffold"))
from paper_style import apply_style, PALETTE
apply_style()
# ... build the figure, then fig.savefig(Path(__file__).with_suffix(".png"))
```

## Worked example

`figures/example-method-comparison/` is a complete, verified reference (a
grouped bar chart). Copy its `.py` and `.tex` as starting points.

## Gotchas (learned by looking at the output)

- **pgfplots `symbolic x coords` silently ignores `enlarge x limits`.** With
  few groups the first group crowds against the y-axis and the leftmost bar
  overlaps the tick labels — no amount of `enlarge x limits` fixes it. Use
  **numeric x positions + explicit `xmin`/`xmax`** (`xtick`/`xticklabels`
  for the category names) for deterministic margins. See the example `.tex`.
- **Grouped bars clip at the axis boundary.** Outer bars that extend past
  the outermost coordinate get cut off (only their value labels remain).
  Set `clip=false` *and* leave margin via `xmin`/`xmax`.
- **Use `axis lines*=left`** (starred) for the L-shaped spine — the
  non-starred `axis y line=left` can place the y-axis at x=0 instead of the
  plot boundary, putting it through your bars.
- **Carlito's digit `0` reads like `O`** at small sizes in the raster
  preview — that's the font, not a rendering bug.
- The matplotlib `<name>.png` and the LaTeX `<name>.pdf-preview.png` are
  different files. The `.png` is the matplotlib deliverable; the
  `-preview.png` is only a raster of the PDF for your eyes.

## Troubleshooting

- `WARNING: Carlito not found` from a Python run → the TeX Live Carlito TTFs
  weren't located. Check `kpsewhich Carlito-Regular.ttf`; install the
  `carlito` TeX package if missing.
- `pdflatex: ... carlito.sty not found` → install the `carlito` package
  (`tlmgr install carlito` on TeX Live).
- `build_pdf.sh: pdftoppm: command not found` → `brew install poppler`
  (macOS) / `apt-get install poppler-utils` (Linux).
- LaTeX `! Package pgfplots Error: ... compat` → bump `compat=` in
  `main.tex` to your pgfplots version (or lower it).

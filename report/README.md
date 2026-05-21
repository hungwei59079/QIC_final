# Few Single-Qubit Measurements Suffice to Certify Any Quantum State — Report

Course writeup based on the paper by Meghal Gupta, William He, and Ryan
O'Donnell ([arXiv:2506.11355](https://arxiv.org/abs/2506.11355)). The
annotated source of the original paper lives in [`../original/`](../original/).

## Note
The pdf file is gitignored in this repository, so beware that it might not be
synchronized with the latest edited .tex files. I will only force add it to
update the compiled output when necessary to prevent bloated commit history.

-- Hung-Wei, Liu

## Files

| File | Role |
|------|------|
| `main.tex` | Top-level document — compile this one |
| `intro.tex` | Section: Introduction (included via `\input`) |
| `algorithm.tex` | Section: Algorithm (included via `\input`) |
| `lower_bound.tex` | Section: Lower Bound (included via `\input`) |

The section files are **not** compiled directly — they are pulled into
`main.tex`. Always build `main.tex`.

## Requirements

A TeX distribution (TeX Live 2023 or newer, or MiKTeX) providing a LaTeX
engine — either `pdflatex` or `xelatex`. The packages used (`tikz`,
`quantikz`, `braket`, `physics`, `mathtools`, `thmtools`, `enumitem`, etc.)
all ship with a full TeX Live installation.

## How to compile

Right now the report has no cross-references or bibliography, so a single
pass is enough:

```sh
pdflatex main.tex      # or: xelatex main.tex
```

Once `\ref`/`\label` are introduced, run the engine **twice** so that
references resolve. If a bibliography is added later, run `bibtex` between
passes — or just use `latexmk`, which handles the cycle automatically:

```sh
latexmk -pdf main.tex      # uses pdflatex; or: latexmk -xelatex main.tex
```

Clean auxiliary files with `latexmk -c` (keeps `main.pdf`) or `latexmk -C`
(also removes `main.pdf`).

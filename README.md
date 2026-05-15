# Few Single-Qubit Measurements Suffice to Certify Any Quantum State

LaTeX source for the paper by Meghal Gupta, William He, and Ryan O'Donnell
([arXiv:2506.11355](https://arxiv.org/abs/2506.11355)).

## Note
The pdf file is gitignored in this repository. I will only force add it to update the comments when necessary, to prevent bloated commit history.

-- Hung-Wei, Liu

## Files

| File | Role |
|------|------|
| `main.tex` | Top-level document — compile this one |
| `pvp.tex` | Body section, included by `main.tex` via `\input` |
| `lower_bound.tex` | Body section, included by `main.tex` via `\input` |
| `style.sty` | Custom package with macros and theorem environments |
| `references.bib` | Bibliography database |
| `main.pdf` | Compiled output (13 pages) |

`pvp.tex` and `lower_bound.tex` are **not** compiled directly — they are
pulled into `main.tex`. Always build `main.tex`.

## Requirements

A TeX distribution (TeX Live 2023 or newer, or MiKTeX) providing:

- A LaTeX engine: `xelatex` or `pdflatex`
- `bibtex` for the bibliography
- `latexmk` (optional but recommended — automates the build cycle)

The packages used (`thmtools`, `quantikz`, `tikz`, `mathtools`, `enumitem`,
etc.) all ship with a full TeX Live installation.

## How to compile

### Recommended: `latexmk` (one command)

`latexmk` runs the engine and `bibtex` as many times as needed to resolve
everything:

```sh
latexmk -xelatex main.tex      # or: latexmk -pdf main.tex   (uses pdflatex)
```

Clean up auxiliary files afterward with:

```sh
latexmk -c        # removes .aux, .log, .out, etc. (keeps main.pdf)
latexmk -C        # also removes main.pdf
```

### Manual build

Cross-references (`\ref`) and citations (`\cite`) need **multiple passes**.
The first pass writes labels and citation keys into `main.aux`; `bibtex`
turns those into `main.bbl`; later passes read them back in. Run all four
steps in order:

```sh
xelatex main.tex      # 1. write .aux (refs/citations unresolved)
bibtex  main          # 2. build .bbl from references.bib  (note: no .tex)
xelatex main.tex      # 3. pull in .bbl, partially resolve refs
xelatex main.tex      # 4. fully resolve all cross-references
```

Substitute `pdflatex` for `xelatex` if you prefer — arXiv builds this paper
with `pdflatex` (see `00README.json`). Both engines work.

> **Why the `??`?** If you run the engine only once, unresolved references
> render as `??` and citations as `[?]` — for example "we prove ?? in ??".
> This is expected mid-build; running the full cycle above resolves them.
> If they persist, run `xelatex` once more.

## VSCode (LaTeX Workshop)

The default single-pass recipe leaves `??` in the output. Use a recipe that
runs the full cycle. Add to your `settings.json`:

```jsonc
{
  "latex-workshop.latex.recipes": [
    {
      "name": "xelatex → bibtex → xelatex × 2",
      "tools": ["xelatex", "bibtex", "xelatex", "xelatex"]
    }
  ],
  "latex-workshop.latex.tools": [
    { "name": "xelatex", "command": "xelatex",
      "args": ["-interaction=nonstopmode", "-synctex=1", "%DOC%"] },
    { "name": "bibtex", "command": "bibtex", "args": ["%DOCFILE%"] }
  ]
}
```

Or simply rely on the built-in `latexmk` recipe, which handles the cycle
automatically.

## Troubleshooting

- **`Citation ... undefined` / `Reference ... undefined` warnings** —
  the build cycle is incomplete. Re-run `bibtex` then `xelatex` twice, or
  use `latexmk`.
- **`inputenc package ignored with utf8 based engines`** — harmless warning;
  `xelatex` is natively UTF-8, so the `inputenc` package does nothing.
- **Stale output after edits** — delete `main.aux` and `main.bbl`, then
  rebuild from scratch, or run `latexmk -C` followed by a fresh build.

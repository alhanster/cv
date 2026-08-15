![LaTeX build](../../workflows/LaTeX%20build/badge.svg)
[![Latest build of AlexHan.pdf](https://img.shields.io/badge/AlexHan.pdf-latest-orange.svg?style=flat)](../gh-action-result/pdflatex/AlexHan.pdf)
[![Latest build of AlexHan-PubsOnly.pdf](https://img.shields.io/badge/AlexHan--PubsOnly.pdf-latest-orange.svg?style=flat)](../gh-action-result/pdflatex/AlexHan-PubsOnly.pdf)

# Alex Han — curriculum vitae LaTeX source

## Layout

Two top-level files, each of which compiles to a PDF:

- [AlexHan.tex](AlexHan.tex) — the full CV.
- [AlexHan-PubsOnly.tex](AlexHan-PubsOnly.tex) — just the publication list, with the contact header.

These pull in three content files, none of which can be compiled on their own:

- [ContactContent.tex](ContactContent.tex) — the contact header.
- [PubsContent.tex](PubsContent.tex) — publications and preprints.
- [PresentationsContent.tex](PresentationsContent.tex) — talks and posters.

[res.cls](res.cls) is the document class. It is **not** part of TeX Live and cannot be
installed with `tlmgr`, so the copy in this repo is the only one — don't delete it.

One thing to watch: `AlexHan-PubsOnly.tex` duplicates the preamble of
`AlexHan.tex` rather than sharing it. Package and metadata changes have to be
made in both files.

## Building

`AlexHan.tex` starts with `\let\nofiles\relax` because `res.cls` suppresses `.aux`
files, which `lastpage` and `hyperref` need. That means the document requires two
passes; `latexmk` handles this automatically.

```bash
latexmk -pdf -halt-on-error -interaction=nonstopmode -file-line-error AlexHan.tex AlexHan-PubsOnly.tex
```

Clean up intermediate files with `latexmk -C`.

On macOS with [BasicTeX](https://www.tug.org/mactex/morepackages.html), the
packages beyond the default install are:

```bash
sudo tlmgr update --self && sudo tlmgr install latexmk lastpage etaremune datetime2 tracklang
```

Every push also triggers the `LaTeX build` GitHub Action, which compiles both
files in a full TeX Live container and force-pushes the PDFs to the orphan
branch `gh-action-result/pdflatex` — that's what the badges above link to.

## Credit

The style, structure, and build setup come from
[duetosymmetry/cv](https://github.com/duetosymmetry/cv) by Leo C. Stein, whose
README invites reuse. `res.cls` is the 1988–89 resume class by Michael DeCorte
(LaTeX2e port by Venkat Krishnamurthy) and retains its original copyright notice.

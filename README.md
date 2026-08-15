![LaTeX build](../../workflows/LaTeX%20build/badge.svg)
[![Latest build of AlexHan.pdf](https://img.shields.io/badge/AlexHan.pdf-latest-orange.svg?style=flat)](../gh-action-result/pdflatex/AlexHan.pdf)

# Alex Han — curriculum vitae LaTeX source

Two files matter:

- [AlexHan.tex](AlexHan.tex) — the entire CV, preamble and content.
- [res.cls](res.cls) — the document class, which is what puts section titles out
  in the left margin. It is **not** part of TeX Live and cannot be installed with
  `tlmgr`, so the copy in this repo is the only one. Don't delete it.

## Building

`AlexHan.tex` starts with `\let\nofiles\relax` because `res.cls` suppresses `.aux`
files, which `lastpage` and `hyperref` need. That means the document requires two
passes; `latexmk` handles this automatically.

```bash
latexmk -pdf -halt-on-error -interaction=nonstopmode -file-line-error AlexHan.tex
```

Clean up intermediate files with `latexmk -C`.

On macOS with [BasicTeX](https://www.tug.org/mactex/morepackages.html), the
packages beyond the default install are:

```bash
sudo /Library/TeX/texbin/tlmgr install latexmk lastpage etaremune datetime2 tracklang
```

Every push also triggers the `LaTeX build` GitHub Action, which compiles the
document in a full TeX Live container and force-pushes the PDF to the orphan
branch `gh-action-result/pdflatex` — that's what the badge above links to.

## Editing

There is a formatting cheat sheet in the comment block at the top of
`AlexHan.tex`. The one piece of bookkeeping to remember: publications are
numbered in descending order, so after adding one, bump `\setcounter{numPubs}`
to the new total or the list won't end at 1.

To edit on [Overleaf](https://www.overleaf.com) instead, zip the repo contents
(not the enclosing folder) and use *New Project → Upload Project*. Set the
compiler to pdfLaTeX; XeLaTeX and LuaLaTeX will fail on `inputenc`. Note that
syncing back to GitHub is an Overleaf premium feature, so on a free account
you'd be moving zips by hand — pick one home for the file and stick with it.

## Credit

The style, structure, and build setup come from
[duetosymmetry/cv](https://github.com/duetosymmetry/cv) by Leo C. Stein, whose
README invites reuse. `res.cls` is the 1988–89 resume class by Michael DeCorte
(LaTeX2e port by Venkat Krishnamurthy) and retains its original copyright notice.

# template-paper

LaTeX paper template for the group: the paper lives in a GitHub repository synced with Overleaf,
anyone can build and review it locally, and every co-author marks their edits in their own colour.

## Start a new paper
1. **Use this template** → new **private** repository `paper-<slug>` in `GCDA-UDL`.
2. Overleaf: **New Project → Import from GitHub** → choose the repository.
3. Local copy: `git clone https://github.com/GCDA-UDL/paper-<slug>.git`.
4. Edit the title, authors, journal and `\revauthor` lines in `main.tex`; keep or remove the letters.

## Working cycle
| Step | Where | What |
|---|---|---|
| 1 | Overleaf | **Menu → GitHub → Push Overleaf changes to GitHub** (if anyone edited there) |
| 2 | local | `git pull --rebase` |
| 3 | local | edit, then `bash tools/build.sh` and check `build/main.pdf` |
| 4 | local | `git commit` and `git push` |
| 5 | Overleaf | **Menu → GitHub → Pull GitHub changes into Overleaf** |

Do not edit the same file in Overleaf and locally at the same time. When both sides changed, Overleaf
pushes its version to a branch named `overleaf-YYYY-MM-DD-HHMM`; merge it into `main` before going on.

## Revision marks (`revnotes.sty`)
Each co-author gets one line in the preamble, `\revauthor{key}{Label}{colour}`, which defines three
commands. For `\revauthor{vitor}{Vitor}{revTeal}`:

| Command | `mode=review` | `mode=changes` | `mode=final` |
|---|---|---|---|
| `\vitoradd{new text}` | coloured | coloured | plain text |
| `\vitordel{old text}` | coloured, struck out | coloured, struck out | removed |
| `\vitornote{reason}` | `[Vitor: reason]` | hidden | hidden |
| `\pending{open item}` | red `[PENDING: …]` | build error | build error |

The mode is the option of `\usepackage[mode=...]{revnotes}` in `main.tex`. `mode=changes` gives the
"revised manuscript with changes marked" that journals ask for; `mode=final` is the submission version,
and it refuses to build while any `\pending` is left. Colours: `revTeal`, `revCyan`, `revBlue`,
`revMahogany`, `revForest`, `revPurple`, `revOrange`, `revMagenta` (red is reserved for `\pending`).
Keep `\…del` to short spans inside one paragraph and wrap a `\cite` inside it in `\mbox{}`.

## Local builds
```bash
bash tools/build.sh                     # build/main.pdf with the mode written in main.tex
bash tools/build.sh --mode final        # build/final/main.pdf, as it will be submitted
bash tools/build.sh letters/response_to_reviewers --open
bash tools/diff.sh sub1                 # build/diff/main_diff.pdf: changes since tag sub1
```
`build.sh` never writes next to the sources and prints a summary: errors, undefined references and
citations, overfull boxes, pages and the number of marks. Needs a TeX distribution (TeX Live or MiKTeX)
and bash (Git Bash on Windows); `diff.sh` also needs `latexdiff-so`.

## Versions
Mark each version sent to a journal with an annotated tag: `sub1` (first submission), `sub1-r1`,
`sub1-r2` (revisions), `sub2` (next journal), `accepted`, `camera-ready`.

## Files
```
main.tex                      paper (elsarticle) with the end matter in place
sections/                     one file per section, so co-authors rarely touch the same file
figures/                      images (\graphicspath)
refs.bib                      bibliography
letters/                      cover letter and response to reviewers
revnotes.sty                  revision marks
tools/build.sh, tools/diff.sh local build and marked-changes PDF
```

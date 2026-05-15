# M3CS PhD Thesis Template

A LaTeX template for PhD theses produced at the **Monash Centre for Consciousness and Contemplative Studies (M3CS)**, Monash University. Configured for the Monash thesis-by-publication format and tested on a thesis submitted in 2026.

> *If this template helped you, consider opening a pull request with improvements so future M3CS students benefit too.*

---

## What you get

- A complete, **compilable-out-of-the-box** thesis skeleton
- **Monash-compliant frontmatter**: title page, copyright, abstract, publications-during-enrolment, thesis-including-published-works declaration, acknowledgements (with the required generative-AI disclosure template)
- **Thesis-by-publication chapter structure** with linking-text scaffolding between paper-chapters
- A pre-tuned **bibliography setup** (biblatex + biber, APA style with sensible thesis-friendly defaults)
- A **table-of-contents style** that's cleaner than the default `book` class TOC (via `tocloft`)
- A pre-set **chapter heading style**, page geometry (with binding offset), and one-and-a-half line spacing per Monash convention
- Sensible **`.gitignore`** for LaTeX projects

## Quick start

```bash
git clone https://github.com/lars-sandved/m3cs-thesis-template.git my-thesis
cd my-thesis
latexmk -pdf main.tex
```

You should get a compiled `main.pdf` with placeholder content. Then:

1. **Edit `main.tex`** — update `\thesistitle`, `\thesisauthor`, `\thesisyear`, etc. (in the "Custom macros" block)
2. **Replace frontmatter placeholders** in `frontmatter/*.tex`
3. **Replace chapter skeletons** with your own content in `chapters/`
4. **Add your bib entries** to `thesis.bib`
5. **Drop figures** into `figures/`

## Using the template with Overleaf

Two routes, in order of convenience:

### Option 1 — Import directly from GitHub (recommended)

Overleaf can clone a public GitHub repo into a new project in one click:

1. Sign in to [Overleaf](https://www.overleaf.com).
2. From the project list page, click **New Project → Import from GitHub**.
3. Paste the repo URL: `https://github.com/lars-sandved/m3cs-thesis-template`
4. Click **Import to Overleaf**. The repo will appear as a new Overleaf project.

The advantage of this route is that Overleaf keeps the link to GitHub. You can pull updates from GitHub and push your own changes back from Overleaf's project menu (**Menu → GitHub → Push to / Pull from GitHub**).

If you'd rather decouple your thesis from this template entirely (most students will), fork the repo on GitHub first, then import your fork into Overleaf.

### Option 2 — Upload as a zip

If you don't want GitHub in the loop at all:

1. From the repo page, click **Code → Download ZIP**.
2. On Overleaf, click **New Project → Upload Project** and select the zip.

### Overleaf project settings

Once the project is in Overleaf, set:

- **Menu → Compiler** → `LaTeX` (default) or `pdfLaTeX`
- **Menu → Main document** → `main.tex`
- **Menu → TeX Live version** → 2023 or later (for `biblatex-apa` to work)

The bibliography uses `biber` (not `bibtex`). Overleaf auto-detects this from the `biblatex` package call in `main.tex` — no manual setting needed.

### Compiling

Click **Recompile** in the top toolbar. The first compile will be slow (it has to run biber and re-pdflatex twice). Subsequent compiles are faster.

If citations show up as `[?]` after a first compile, click the arrow next to **Recompile** and select **Recompile from scratch**.

### Tips

- **Track changes with co-authors**: Overleaf's track changes feature is paid-only. If you want free track changes, use the GitHub workflow (Option 1) and use pull requests on your fork.
- **Sharing**: from **Menu → Share**, you can invite supervisors as collaborators. Free Overleaf accounts allow one collaborator per project; paid accounts allow more.
- **Backups**: even on Overleaf, periodically push to GitHub. Overleaf's history feature is limited on free accounts.

## File layout

```
m3cs-thesis-template/
├── main.tex                   # preamble + document structure
├── thesis.bib                 # bibliography database
├── README.md                  # this file
├── .gitignore                 # LaTeX build artefacts
│
├── frontmatter/
│   ├── titlepage.tex
│   ├── copyright.tex
│   ├── abstract.tex
│   ├── publications.tex       # publications during enrolment
│   ├── declaration.tex        # thesis-by-publication declaration form
│   └── acknowledgements.tex   # personal thanks + AI-use disclosure
│
├── chapters/
│   ├── 01_introduction.tex
│   ├── 02_chapter_template.tex   # template for paper-chapters
│   └── 99_conclusion.tex
│
└── figures/
    └── monash_logo.png        # used on the title page
```

## Conventions

### Citations

Citation style is APA via `biblatex` with these thesis-friendly options set in `main.tex`:

- `maxcitenames=2` — papers with 3+ authors render as `Smith et al.` in-text
- `uniquename=false` — no disambiguating initials when surnames collide (last names only)
- `uniquelist=false` — no expansion of author lists past `maxcitenames` to disambiguate against other entries
- `maxbibnames=99` — full author lists in the bibliography

In-text citation commands (from natbib via biblatex):

| Command | Output |
|---|---|
| `\citet{key}` | Author (Year) |
| `\citep{key}` | (Author, Year) |
| `\citep[p.~42]{key}` | (Author, Year, p. 42) |
| `\citep[see][]{key}` | (see Author, Year) |
| `\citep{key1,key2}` | (Author1, Year; Author2, Year) |
| `\citeauthor{key}` | Author |
| `\citeyear{key}` | Year |

### Cross-references

Use `\Cref{...}` (capitalised) and `\cref{...}` (lowercase) from the `cleveref` package — they auto-detect whether you're referring to a chapter, section, figure, table, or equation.

```latex
See \Cref{ch:introduction} for context, \Cref{fig:results} for the data,
and \Cref{sec:methods} for the analysis details.
```

### Figures

Place all images in `figures/`. Reference by filename (the `\graphicspath{{figures/}}` directive in `main.tex` handles the path):

```latex
\begin{figure}[htbp]
    \centering
    \includegraphics[width=0.7\linewidth]{my_figure.png}
    \caption{A self-explanatory caption.}
    \label{fig:my-figure}
\end{figure}
```

The float specifier `[htbp]` lets LaTeX place the figure flexibly (here, top, bottom, page). Use `[H]` from the `float` package only if you really need the figure exactly where it's source-positioned.

For figures spanning a full page or wider than text width: `width=\textwidth`. For most thesis figures, `width=0.7\linewidth` reads well without overpowering the text.

### Tables

Use `booktabs` rules (`\toprule`, `\midrule`, `\bottomrule`) — never `\hline` — for publication-quality tables.

For tables that need flexible column widths (e.g., the contributions table in `declaration.tex`), use `tabularx` with `X` columns:

```latex
\begin{tabularx}{\linewidth}{@{}c >{\raggedright\arraybackslash}X l r@{}}
    ...
\end{tabularx}
```

`X` columns share the leftover width after fixed-width columns are sized.

### Lists

Use `enumitem` options for fine control:

```latex
\begin{itemize}[label={},leftmargin=0pt,itemsep=0.8em]
    % no bullet markers, flush left, generous spacing
\end{itemize}
```

## Monash thesis requirements (2026, check current)

The following are required as of 2026 — **confirm against current Monash policy** before submission:

1. **Originality declaration** — boilerplate paragraph at the top of `declaration.tex`. Do not edit.
2. **Thesis-including-published-works statement** — second paragraph of `declaration.tex`. Edit to describe your thesis theme.
3. **Per-chapter contributions table** — list each paper-chapter, its publication status, and your % contribution.
4. **Student signature + date** — handwritten, scanned. Save as `figures/student_signature.jpg` and uncomment the `\includegraphics` line in `declaration.tex`.
5. **Supervisor signature + date** — same arrangement, in `figures/supervisor_signature.png`.
6. **Generative AI disclosure** — if you used AI tools in preparing the thesis, the disclosure goes in the acknowledgements (template provided in `acknowledgements.tex`).

## Customisation tips

### Change the TOC look

Open `main.tex` and look for the **"Table of contents styling"** block. The current settings:

- Roman (non-bold) chapter entries
- Generous vertical spacing between chapters (`1.2em`)
- Dotted leader to right-aligned page numbers
- Heading: `\Huge\bfseries`, left-aligned

To restore the default `book` class TOC, comment out the `\usepackage{tocloft}` line and the redefinitions below it.

### Change the chapter heading style

Open `main.tex` and edit the `\titleformat{\chapter}` block. The current style is "display" with the chapter number on its own line above the title.

### Switch citation style

Replace `style=apa` in the `\usepackage[...]{biblatex}` call with another biblatex style (e.g. `numeric`, `nature`, `chicago-authordate`).

### Change the page margins

Edit the `\usepackage[...]{geometry}` line in `main.tex`. The current values (`margin=2.5cm,bindingoffset=1cm`) follow Monash binding requirements.

## Compilation

The thesis uses **biber** as the bibliography backend (not bibtex). `latexmk -pdf` will handle this automatically. Manual compilation:

```bash
pdflatex main
biber main
pdflatex main
pdflatex main
```

In Overleaf: project settings → "Latexmk" compiler, "Biber" bibliography compiler.

## Acknowledgements

This template was extracted from the thesis "*A physics of contemplative phenomenology*" by Lars Sandved-Smith (Monash, 2026), supervised by Professor Jakob Hohwy. Improvements and contributions from future M3CS students are welcome via pull request.

## License

MIT — use freely, modify freely, no warranty. See `LICENSE` (forthcoming).

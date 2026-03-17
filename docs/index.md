# UHHthesis

The **UHHthesis** ecosystem provides ready-to-use thesis templates for
Bachelor’s and Master’s theses at the University of Hamburg (UHH),
designed for students in the MIN faculty (primarily Biology). Two tools
are available: an **R package** built on bookdown and R Markdown, and a
**Quarto extension** for the modern Quarto publishing framework. Both
tools produce PDF and Word output that conforms to the official UHH/MIN
submission standards, and aim to encourage reproducible research.

> **Thesis Guide:** General guidance on planning, structuring, and
> writing your BSc or MSc thesis — from the initial research question to
> the final submission: [English
> (PDF)](https://uham-bio.github.io/UHHthesis/) \| [Deutsch
> (PDF)](https://uham-bio.github.io/UHHthesis/)

------------------------------------------------------------------------

## R package – UHHthesis

The **UHHthesis** R package wraps [bookdown](https://bookdown.org/) and
provides four R Markdown output format functions. It creates a
multi-file project with pre-filled chapters, a front matter, and a
bibliography folder. It is the right choice if your workflow is entirely
in R and RStudio.

| Template | Language | Output | Function |
|:---|:---|:---|:---|
| PDF thesis | German | PDF (LaTeX) | [`thesis_pdf_de()`](https://uham-bio.github.io/UHHthesis/reference/thesis_pdf_de.md) |
| PDF thesis | English | PDF (LaTeX) | [`thesis_pdf_en()`](https://uham-bio.github.io/UHHthesis/reference/thesis_pdf_de.md) |
| Word thesis | German | DOCX | [`thesis_word_de()`](https://uham-bio.github.io/UHHthesis/reference/thesis_word_de.md) |
| Word thesis | English | DOCX | [`thesis_word_en()`](https://uham-bio.github.io/UHHthesis/reference/thesis_word_de.md) |

------------------------------------------------------------------------

## Quarto Extension – quarto-UHHthesis

The [**quarto-UHHthesis**](https://github.com/uham-bio/quarto-UHHthesis)
extension delivers the same UHH thesis templates for the
[Quarto](https://quarto.org/) framework. It supports R, Python, and
Julia in a single document, works in VS Code, Positron, RStudio, and any
text editor, and is the **recommended choice for new thesis projects**.

------------------------------------------------------------------------

## Prerequisites

| Tool | Required for | Min. version | How to get |
|:---|:---|:---|:---|
| R | R Markdown templates | ≥ 4.2 | [cran.r-project.org](https://cran.r-project.org) |
| RStudio or Positron | both | current | [posit.co/downloads](https://posit.co/downloads/) |
| bookdown | R Markdown | any | `install.packages(“bookdown”)` |
| LaTeX / TinyTeX | PDF output (both tools) | any | [`tinytex::install_tinytex()`](https://rdrr.io/pkg/tinytex/man/install_tinytex.html) |
| Quarto | Quarto extension | ≥ 1.3 | [quarto.org](https://quarto.org/docs/get-started/) |

------------------------------------------------------------------------

## Quick installation guide

### R package

``` R
# Using the package 'pak' (recommended)
if (!require("pak")) install.packages("pak")
pak::pak("uham-bio/UHHthesis")

# Alternatively, using 'remotes'
if (!require("remotes")) install.packages("remotes")
remotes::install_github("uham-bio/UHHthesis")
```

For step-by-step instructions including prerequisites and project
creation, see the Getting Started guides: [Getting Started
(EN)](https://uham-bio.github.io/UHHthesis/articles/uhhthesis-getting-started-en.html)
\| [Erste Schritte
(DE)](https://uham-bio.github.io/UHHthesis/articles/uhhthesis-getting-started-de.html)

### Quarto extension

Open a terminal in the folder where you want to create your thesis
project, then run one of the following:

``` R
# English template
quarto use template uham-bio/quarto-UHHthesis/en

# German template
quarto use template uham-bio/quarto-UHHthesis/de
```

For step-by-step instructions including prerequisites and metadata
configuration, see the Getting Started guides: [Getting Started
(EN)](https://uham-bio.github.io/UHHthesis/articles/quarto-getting-started-en.html)
\| [Erste Schritte
(DE)](https://uham-bio.github.io/UHHthesis/articles/quarto-getting-started-de.html)

------------------------------------------------------------------------

## Project structure

### R Markdown (UHHthesis)

``` R
your-thesis/
├── index.Rmd              # Main file — the ONLY file to knit
├── _bookdown.yml          # Chapter order and output settings
├── template.tex           # LaTeX template (do not edit)
├── chapter/               # One .Rmd file per thesis chapter
├── prelim/                # Abstract, Zusammenfassung, Abbreviations
├── bib/                   # references.bib + citation style (.csl)
├── data/                  # Data files
├── images/                # Logos and external images
└── thesis-output/         # Generated PDF / DOCX (created on first knit)
```

### Quarto (quarto-UHHthesis)

``` R
your-thesis/
├── _quarto.yml            # Main configuration (title, author, formats, bibliography)
├── index.qmd              # Title page metadata and abstract includes
├── _extensions/UHHthesis/ # Format extension (do not edit)
├── chapter/               # One .qmd file per thesis chapter
├── prelim/                # Abstract, Zusammenfassung, Abbreviations
├── bib/                   # references.bib + citation style (.csl)
├── data/                  # Data files
├── images/                # Logos and external images
└── thesis-output/         # Generated PDF / DOCX (created on first render)
```

------------------------------------------------------------------------

## Documentation

Full documentation is available on the [UHHthesis
website](https://uham-bio.github.io/UHHthesis/):

**R Markdown (UHHthesis)**

- [Getting Started
  (EN)](https://uham-bio.github.io/UHHthesis/articles/uhhthesis-getting-started-en.html)
  — installation, creating your project, project structure, rendering
- [Erste Schritte
  (DE)](https://uham-bio.github.io/UHHthesis/articles/uhhthesis-getting-started-de.html)
- [Writing Your Thesis
  (EN)](https://uham-bio.github.io/UHHthesis/articles/uhhthesis-tutorial-en.html)
  — YAML, R Markdown, figures, tables, equations, citations,
  cross-references, troubleshooting
- [Abschlussarbeit schreiben
  (DE)](https://uham-bio.github.io/UHHthesis/articles/uhhthesis-tutorial-de.html)
- [Template
  Gallery](https://uham-bio.github.io/UHHthesis/articles/gallery.html)

**Quarto Extension (quarto-UHHthesis)**

- [Getting Started
  (EN)](https://uham-bio.github.io/UHHthesis/articles/quarto-getting-started-en.html)
- [Erste Schritte
  (DE)](https://uham-bio.github.io/UHHthesis/articles/quarto-getting-started-de.html)
- [Writing Guide
  (EN)](https://uham-bio.github.io/UHHthesis/articles/quarto-writing-guide-en.html)
- [Schreibanleitung
  (DE)](https://uham-bio.github.io/UHHthesis/articles/quarto-writing-guide-de.html)

## Useful resources

- Quarto
  - Quarto documentation\](<https://quarto.org/docs/guide/>)
  - [Quarto books](https://quarto.org/docs/books/)
  - [Quarto
    cross-references](https://quarto.org/docs/authoring/cross-references.html)
  - [Quarto
    citations](https://quarto.org/docs/authoring/footnotes-and-citations.html)
- R Markdown
  - The official [R Markdown
    documentation](https://rmarkdown.rstudio.com/lesson-1.html) from
    Posit
  - [R Markdown
    Cheatsheet](https://rstudio.github.io/cheatsheets/rmarkdown.pdf)
  - The online book [R Markdown: The Definitive
    Guide](https://yihui.org/rmarkdown/) by Yihui Xie, J. J. Allaire,
    and Garrett Grolemund
- Bookdown
  - The online book [bookdown: Authoring Books and Technical Documents
    with R Markdown](https://yihui.org/bookdown/) by Yihui Xie
- LaTeX
  - [TinyTeX LaTeX distribution](https://yihui.org/tinytex/)
  - The official [LaTeX help and
    documentation](https://www.latex-project.org/help/documentation/)
  - The [overleaf](https://www.overleaf.com/learn) documentation
- Git
  - The online book [Happy Git and GitHub for the
    useR](https://happygitwithr.com/) is a novice-friendly guide to
    getting starting with using Git with R and RStudio.
- Citations
  - [CSL Style
    Repository](https://github.com/citation-style-language/styles)
  - [Zotero reference manager](https://www.zotero.org/)
- [flextable book](https://ardata-fr.github.io/flextable-book/)

------------------------------------------------------------------------

## Author

**Saskia A. Otto** University of Hamburg · Department of Biology ·
Institute of Marine Ecosystem and Fisheries Science ·
[GitHub](https://github.com/saskiaotto) ·
[Website](https://www.biologie.uni-hamburg.de/forschung/marine-oekosystemdynamik/mitarbeiter/otto-saskia.html)

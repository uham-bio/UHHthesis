
<!-- README.md is generated from README.Rmd. Please edit that file -->

# UHHthesis <img src="vignettes/images/UHHthesis_logo.png" align="right" width="110" height="110"/>

[![Author: Saskia
Otto](https://img.shields.io/badge/author-Saskia%20Otto-blue)](https://github.com/saskiaotto)
[![License:
MIT](https://img.shields.io/badge/license-MIT-blue)](LICENSE)

This package is a customized version of the original
[thesisdown](https://github.com/ismayc/thesisdown) package developed by
Chester Ismay for the Reed College. It creates a *bookdown* project
structure with German or English LaTeX and Word thesis templates from
the University of Hamburg (UHH), which conform to the submission
standards of the MIN faculty for Bachelor and Master theses. The package
is primarily designed for the Biology Department but can be used by any
UHH student. It aims to encourage students to conduct reproducible
research using simple Markdown syntax while embedding all of the R code
to produce plots and analyses as well.

> UPDATE: **Looking for the Quarto version?** Want to combine R with
> Python or Julia, work in Positron, VS Code, or another editor of your
> choice, and use the latest publishing framework? Then check out our
> latest tool
> [quarto-UHHthesis](https://github.com/uham-bio/quarto-UHHthesis) — the
> recommended choice for all new thesis projects.

The *UHHthesis* package contains currently four templates:

| Template    | Language | Output      | Function           |
|:------------|:---------|:------------|:-------------------|
| PDF thesis  | German   | PDF (LaTeX) | `thesis_pdf_de()`  |
| PDF thesis  | English  | PDF (LaTeX) | `thesis_pdf_en()`  |
| Word thesis | German   | DOCX        | `thesis_word_de()` |
| Word thesis | English  | DOCX        | `thesis_word_en()` |

## Installation

``` r
# Using the package 'pak' (recommended)
if (!require("pak")) install.packages("pak")
pak::pak("uham-bio/UHHthesis")

# Alternatively, using 'remotes'
if (!require("remotes")) install.packages("remotes")
remotes::install_github("uham-bio/UHHthesis")
```

## Prerequisites

- **R** ≥ 4.2 and **RStudio** (or Positron)
- **bookdown**: `install.packages("bookdown")`
- **LaTeX** for PDF output — the easiest option is
  [TinyTeX](https://yihui.org/tinytex/):

``` r
install.packages("tinytex")
tinytex::install_tinytex()
```

## Documentation

Full documentation, tutorials, and the template gallery are available on
the **[UHHthesis website](https://uham-bio.github.io/UHHthesis/)**.

# Convert R Markdown to a PDF/LaTeX or thesis document in German and English

These functions serves as wrappers for the bookdown function
[`pdf_book`](https://pkgs.rstudio.com/bookdown/reference/pdf_book.html),
with a custom Pandoc LaTeX template and different knitr default values
(e.g., `fig.align = "center"`). It is called from the initial R Markdown
template file, which should be named `index.Rmd`. The functions are
based on the `thesis_pdf` function of the
[thesisdown](https://github.com/ismayc/thesisdown) package.

## Usage

``` r
thesis_pdf_de(
  toc = TRUE,
  toc_depth = 5,
  highlight = "default",
  latex_engine = "pdflatex",
  pandoc_args = NULL,
  ...
)

thesis_pdf_en(
  toc = TRUE,
  toc_depth = 5,
  highlight = "default",
  latex_engine = "pdflatex",
  pandoc_args = NULL,
  ...
)
```

## Arguments

- toc:

  logical; `TRUE` to include a table of contents in the output.

- toc_depth:

  integer; Depth of headers to include in table of contents. Default set
  to 5.

- highlight:

  character; syntax highlighting style. Supported styles include
  "default", "tango", "pygments", "kate", "monochrome", "espresso",
  "zenburn", and "haddock". Pass `NULL` to prevent syntax highlighting.

- latex_engine:

  character; LaTeX engine for producing PDF output. Default is
  "pdflatex" as it works well with the German Umlaute.

- pandoc_args:

  Additional command line options to pass to pandoc.

- ...:

  Additional parameters to pass to
  [`pdf_book`](https://pkgs.rstudio.com/bookdown/reference/pdf_book.html).

## Value

A modified
[`pdf_document`](https://pkgs.rstudio.com/rmarkdown/reference/pdf_document.html)
based on the UHH LaTeX template.

## Examples

``` r
if (FALSE) { # \dontrun{
 # put in YAML header:
 output: UHHthesis::thesis_pdf_de
} # }
```

# Convert R Markdown to a Word (docx) thesis document in German and English

These functions serve as wrappers for the bookdown function
[`word_document2`](https://pkgs.rstudio.com/bookdown/reference/html_document2.html),
with a custom Pandoc Word template and different knitr default values
(e.g., `dpi = 144`). It is called from the initial R Markdown template
file, which should be named `index.Rmd`.

## Usage

``` r
thesis_word_de(
  toc = TRUE,
  toc_depth = 5,
  number_sections = TRUE,
  highlight = "default",
  reference_docx = "uhh-template.docx",
  dpi = 144,
  pandoc_args = NULL,
  ...
)

thesis_word_en(
  toc = TRUE,
  toc_depth = 5,
  number_sections = TRUE,
  highlight = "default",
  reference_docx = "uhh-template.docx",
  dpi = 144,
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

- number_sections:

  logical; whether to number section headers: if TRUE (default),
  figure/table numbers will be of the form X.i, where X is the current
  first-level section number, and i is an incremental number (the i-th
  figure/table); if FALSE, figures/tables will be numbered sequentially
  in the document from 1, 2, ..., and you cannot cross-reference section
  headers in this case.

- highlight:

  character; syntax highlighting style. Supported styles include
  "default", "tango", "pygments", "kate", "monochrome", "espresso",
  "zenburn", and "haddock". Pass `NULL` to prevent syntax highlighting.

- reference_docx:

  character; use the specified file as a style reference in producing a
  docx file. The 'uhh-template.docx' template implements most of the
  standard requirement at the UHH biology department. If you prefer
  another template, pass the file name to this argument or simply use
  'default' to use your standard Word template.

- dpi:

  integer; the resolution of the output figures, default is 144 dots per
  inch.

- pandoc_args:

  Additional command line options to pass to pandoc.

- ...:

  Additional parameters to pass to
  [`pdf_book`](https://pkgs.rstudio.com/bookdown/reference/pdf_book.html).

## Value

A modified
[`word_document`](https://pkgs.rstudio.com/rmarkdown/reference/word_document.html)
based on the UHH Word template.

## Examples

``` r
if (FALSE) { # \dontrun{
 # put in YAML header:
 output: UHHthesis::thesis_word_de
} # }
```

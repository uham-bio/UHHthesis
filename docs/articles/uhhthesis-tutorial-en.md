# Writing Your Thesis with UHHthesis

This reference covers everything you need once your thesis project is
set up. For installation and project creation, see [Getting
Started](https://uham-bio.github.io/UHHthesis/articles/uhhthesis-getting-started-en.md).

> **Golden rule:** Only ever knit `index.Rmd`. Never knit individual
> chapter files.

## 1. YAML Configuration

The YAML header at the top of `index.Rmd` controls the title page,
bibliography, and document options. Here is the default header with
explanations:

``` yaml
---
# --- Title page fields ---
type: "Bachelor's / Master's Thesis"   # Change to your thesis type
title: "Thesis title"                  # Your thesis title
author: "Mandy Mustermann"            # Your name
birth: "31. January 1970"              # Your date of birth
degree_program: "Biology"              # Your degree program
submit_date: "1. April 2000"           # Submission date
advisor1: "Prof. A. First-Aid"         # First advisor
advisor2: "Prof. Z. Itwillbeallright"  # Second advisor

# --- Abstract, summary, abbreviations ---
# These pull content from the prelim/ folder automatically.
# Remove the 'abbreviations' lines if you do not need them.
abstract: |
  
zusammenfassung: |
  
abbreviations: |
  

# --- Optional lists ---
lot: true                              # List of tables (set false to remove)
lof: true                              # List of figures (set false to remove)
link-citations: true                   # Clickable citation links

# --- Bibliography ---
bibliography: [bib/references.bib, bib/packages.bib]
csl: bib/sage-harvard.csl             # Citation style file

# --- Formatting ---
space_between_paragraphs: true         # Extra space between paragraphs
header-includes:
    - \usepackage{setspace}\onehalfspacing  # Line spacing

# --- Build settings (do not change) ---
knit: "bookdown::render_book"
output: UHHthesis::thesis_pdf_en
---
```

### Line spacing options

Change the `header-includes` line to adjust line spacing:

- `\singlespacing` — single spacing
- `\onehalfspacing` — 1.5 spacing (default, recommended)
- `\doublespacing` — double spacing

------------------------------------------------------------------------

## 2. R Markdown Basics

R Markdown files (`.Rmd`) combine plain text with R code. Here is a
quick reference for the most common formatting elements.

### Headings

``` markdown
# Chapter Title          (Level 1 -- used for main chapters)
## Section               (Level 2)
### Subsection           (Level 3)
#### Sub-subsection      (Level 4)
```

### Unnumbered sections

Add `{-}` after the heading to suppress numbering:

``` markdown
# Acknowledgements {-}
```

This is already used in the abstract and abbreviations files.

### Text formatting

``` markdown
**bold text**
*italic text*
~~strikethrough~~
`inline code`
```

### Lists

Unordered list:

``` markdown
- Item one
- Item two
  - Sub-item (indent with two spaces)
```

Ordered list:

``` markdown
1. First item
2. Second item
3. Third item
```

### Block quotes

``` markdown
> This is a block quote. It will be indented in the output.
```

### Comments

Content inside HTML comments is not rendered:

``` markdown
<!-- This text will not appear in the output. -->
```

### Page breaks

Force a new page (PDF only):

``` markdown
\newpage
```

### Footnotes

``` markdown
This sentence has a footnote^[This is the footnote text.].
```

### Horizontal rules

``` markdown
---
```

### Inline R code

You can embed R expressions directly in your text using backtick-r
syntax. The expression is evaluated during knitting and replaced with
the result:

``` markdown
The dataset contains 32 observations.
The mean fuel economy is 20.1 miles per gallon.
```

This is particularly useful in the Material and Methods chapter for
citing software versions automatically (see [Citations and
Bibliography](#id_10-citations-and-bibliography)).

### Code chunks

R code chunks are fenced with triple backticks and `{r}`. The chunk
label and options are set inside the curly braces:

```` default
```{r chunk-label, echo=FALSE, warning=FALSE}
# Your R code here
```
````

Here is an overview of commonly used chunk options:

| Option | Description | Default |
|:---|:---|:---|
| `echo` | Show the R code in the output | `TRUE` |
| `eval` | Execute the code | `TRUE` |
| `warning` | Show warning messages | `TRUE` |
| `message` | Show messages (e.g., from [`library()`](https://rdrr.io/r/base/library.html)) | `TRUE` |
| `include` | Include the chunk and its output at all | `TRUE` |
| `cache` | Cache results to speed up re-knitting | `FALSE` |
| `results` | How to display text results (`"asis"` for raw output) | `"markup"` |
| `fig.cap` | Figure caption (required for numbering) | – |
| `fig.width` | Figure width in inches | `7` |
| `fig.height` | Figure height in inches | `5` |
| `out.width` | Display width in the document | – |

The defaults for `echo`, `warning`, and `message` are typically set
globally in the setup chunk of `index.Rmd` (via
`knitr::opts_chunk$set()`), so you do not need to repeat them in every
chunk.

For figure and table-specific options, see [Figures](#id_3-figures) and
[Tables](#id_4-tables).

------------------------------------------------------------------------

## 3. Figures

### External images

To include an image file (photo, diagram, map), place it in the
`images/` folder and use
[`knitr::include_graphics()`](https://rdrr.io/pkg/knitr/man/include_graphics.html)
inside a code chunk. The chunk label becomes the cross-reference key.

```` default
```{r location, fig.cap="Location of the sampling site on the Elbe estuary.", out.width="80%"}
knitr::include_graphics("images/sampling_site.jpg")
```
````

- `fig.cap` — sets the figure caption (required for numbering and
  cross-referencing).
- `out.width` — controls the display width as a percentage of the page.

Reference this figure in your text with `\@ref(fig:location)`, which
renders as “Figure 1” (or the appropriate number).

### R-generated figures

You can create figures directly with R code. The chunk label and
`fig.cap` work the same way as for external images.

**Scatter plot with ggplot2:**

```` default
```{r scatter-fig, fig.cap="Relationship between horsepower and fuel economy.", out.width='100%'}
library(ggplot2)
ggplot(mtcars, aes(x = hp, y = mpg)) +
  geom_point() +
  geom_smooth(method = "lm", col = "red", se = FALSE) +
  labs(x = "Gross Horsepower", y = "Miles Per Gallon") +
  theme_minimal()
```
````

**Boxplot with ggplot2:**

```` default
```{r boxplot-fig, fig.cap="Fuel economy differences between transmission types.", out.width='60%', fig.height=3}
library(ggplot2)
ggplot(mtcars, aes(x = factor(am, labels = c("Automatic", "Manual")), y = mpg)) +
  geom_boxplot(fill = "steelblue", alpha = 0.7) +
  labs(x = "Transmission", y = "Miles Per Gallon") +
  theme_minimal()
```
````

### Useful chunk options for figures

| Option       | Description                           | Example                 |
|:-------------|:--------------------------------------|:------------------------|
| `fig.cap`    | Caption text (required for numbering) | `fig.cap="My caption."` |
| `out.width`  | Display width                         | `out.width='80%'`       |
| `fig.height` | Height in inches                      | `fig.height=4`          |
| `fig.width`  | Width in inches                       | `fig.width=6`           |
| `fig.align`  | Alignment (`center`, `left`, `right`) | `fig.align='center'`    |

### Cross-referencing figures

Use `\@ref(fig:chunk-label)` in your text:

``` markdown
As shown in Figure \@ref(fig:scatter-fig), there is a negative relationship between horsepower and fuel economy.
```

**Important:** Chunk labels must not contain underscores. Use hyphens
instead (e.g., `scatter-fig`, not `scatter_fig`).

------------------------------------------------------------------------

## 4. Tables

### R Markdown tables (manual)

You can write simple tables directly in Markdown. The caption is placed
above or below the table with `Table: ...`, and the label is set with
LaTeX notation `\label{tab:name}`.

``` markdown
Table: Description of variables used in the analysis.\label{tab:variables}

| Variable     | Unit             | Description    |
|:-------------|:----------------:|---------------:|
| left-aligned | center-aligned   | right-aligned  |
| Temperature  | degrees C        | Surface temp   |
| Salinity     | PSU              | Practical sal  |
```

Cross-reference R Markdown tables with LaTeX notation:
`\ref{tab:variables}`.

**Note:** Since these use LaTeX `\label` / `\ref`, the syntax differs
from R-generated tables (which use `\@ref(tab:label)`).

### kableExtra (recommended for PDF output)

The [`knitr::kable()`](https://rdrr.io/pkg/knitr/man/kable.html)
function combined with the **kableExtra** package produces
well-formatted tables with minimal effort. The caption is set inside the
`kable()` function.

```` default
```{r kable-example}
df <- mtcars[1:5, 1:4]
kable(df, booktabs = TRUE,
  caption = "A table with kableExtra formatting.") |>
  kable_styling(position = "center", font_size = 9) |>
  footnote(general = "Data source: mtcars dataset.")
```
````

You can add grouped headers, striped rows, and other styling:

```` default
```{r kable-grouped}
df <- mtcars[1:5, 1:6]
kable(df, booktabs = TRUE,
  caption = "Motor car data with grouped column headers.") |>
  kable_styling(position = "center", font_size = 9) |>
  add_header_above(c(" " = 1, "Performance" = 2, "Design" = 2,
    "Weight" = 1, "Speed" = 1))
```
````

Cross-reference with `\@ref(tab:kable-example)`.

### flextable (works for both PDF and Word)

The **flextable** package produces tables that render correctly in both
PDF andWord output. This makes it a good choice if you plan to generate
both formats.

```` default
```{r flex-example}
library(flextable)
df <- mtcars[1:5, 1:4]
ft <- flextable(df) |>
  set_caption(caption = "A table with flextable.") |>
  theme_booktabs() |>
  autofit()
ft
```
````

### A note on the gt package

The **gt** package (<https://gt.rstudio.com/>) is a modern and powerful
tablepackage. However, it currently has limited support for bookdown PDF
output.If your primary output is PDF, stick with **kableExtra** or
**flextable**. If you are only producing Word or HTML output, **gt** is
worth considering.

### Which package should I use?

| Package | PDF | Word | Same code for both? | Best for |
|:---|:--:|:--:|:--:|:---|
| Markdown | ✓ | ✓ | ✓ | Small, hand-written tables |
| kableExtra | ✓ | ✗ | – | PDF output, many online examples |
| **flextable** | **✓** | **✓** | **✓** | **Best for dual PDF/Word output** |
| gt | limited | ✓ | mostly | Modern API, rich formatting |

**Recommendation:** If you only produce PDF output, **kableExtra** is a
solid choicewith extensive documentation. If you plan to generate both
PDF and Word, use **flextable** — it produces consistent output in both
formats without extra code.

### Cross-referencing tables — summary

| Table type | How to set caption | Cross-reference syntax |
|:---|:---|:---|
| R Markdown table | `Table: Caption.\label{tab:name}` | `\ref{tab:name}` |
| kable / kableExtra | `caption = "..."` inside `kable()` | `\@ref(tab:chunk-label)` |
| flextable | `set_caption(caption = "...")` | `\@ref(tab:chunk-label)` |

------------------------------------------------------------------------

## 5. Mathematical Equations and Chemical Formulas

### Inline math

Wrap LaTeX math in single dollar signs:

``` markdown
The famous equation $E = mc^2$ relates energy and mass.
```

**Important:** Do not leave a space between the `$` and the math
content.

### Display equations (unnumbered)

Use double dollar signs for centered, display-style equations:

``` markdown
$$\bar{X} = \frac{\sum_{i=1}^n X_i}{n}$$
```

### Numbered equations

Use the LaTeX `equation` environment with a label for cross-referencing:

``` markdown
\begin{equation}
  \bar{X} = \frac{\sum_{i=1}^n X_i}{n}
  (\#eq:mean)
\end{equation}
```

Reference it with `\@ref(eq:mean)`, which renders as “Equation (1)” (or
the appropriate number).

**More examples:**

``` markdown
\begin{equation}
  f_{Y}(y) = \frac{1}{\sqrt{2\pi}} \exp\left\{ -\frac{y^2}{2} \right\}
  (\#eq:density-norm)
\end{equation}
```

Only number equations that you actually cross-reference in the text.

### Chemical formulas

Use `$\mathrm{...}$` to typeset chemical formulas in upright
(non-italic) font:

| Markdown                      | Renders as   |
|:------------------------------|:-------------|
| `$\mathrm{H_2O}$`             | H2O          |
| `$\mathrm{CO_2}$`             | CO2          |
| `$\mathrm{CH_4}$`             | CH4          |
| `$\mathrm{Fe_2^{2+}Cr_2O_4}$` | Fe2(2+)Cr2O4 |

Useful symbols for chemistry:

| Symbol               | Markdown               |
|:---------------------|:-----------------------|
| Superscript (charge) | `$\mathrm{O^-}$`       |
| Subscript            | `$\mathrm{CH_4}$`      |
| Reaction arrow       | `$\longrightarrow$`    |
| Reversible arrow     | `$\rightleftharpoons$` |
| Resonance arrow      | `$\leftrightarrow$`    |
| Delta                | `$\Delta$`             |
| Bullet (hydrate)     | `$\bullet$`            |

### Reaction equations

For numbered reaction equations, use the `equation` environment just
like for math:

``` markdown
\begin{equation}
  \mathrm{C_6H_{12}O_6 + 6O_2} \longrightarrow \mathrm{6CO_2 + 6H_2O}
  (\#eq:reaction)
\end{equation}
```

Reference with `\@ref(eq:reaction)`.

For unnumbered inline reactions, use dollar signs:

``` markdown
$\mathrm{NH_4Cl_{(s)}} \rightleftharpoons \mathrm{NH_{3(g)} + HCl_{(g)}}$
```

------------------------------------------------------------------------

## 6. Citations and Bibliography

### How it works

The UHHthesis template uses **BibTeX** for managing references. You
store your references in `bib/references.bib`, and Pandoc’s built-in
citation processor (**citeproc**) automatically formats them according
to the citation style defined in the YAML header (`.csl` file).

**Where does the bibliography appear?** The file
`chapter/96-references.Rmd` controls where the reference list is placed.
You do **not** write anything in this file — it works automatically. The
key element inside is a special HTML tag:

``` html
<div id="refs"></div>
```

This tells Pandoc: “Insert the generated bibliography **here**.” Without
this tag, the bibliography would be appended at the very end of the
document (after the appendix and declaration). The file also contains
LaTeX formatting commands for hanging indents in PDF output. **Do not
modify or delete this file.**

### BibTeX entry format

Each reference in `references.bib` looks like this:

``` bibtex
@article{May1976,
  author  = {May, R. M.},
  title   = {Simple mathematical models with very complicated dynamics},
  journal = {Nature},
  volume  = {261},
  number  = {5560},
  pages   = {459-467},
  year    = {1976},
  doi     = {10.1038/261459a0}
}
```

The first field after the opening brace (`May1976`) is the **citation
key** — this is what you use to cite the reference in your text.

Common entry types: `@article`, `@book`, `@inproceedings`, `@phdthesis`,
`@techreport`, `@manual`, `@misc`.

### Citation syntax

| Markdown | Output | Use case |
|:---|:---|:---|
| `@May1976` | May (1976) | Author as part of the sentence |
| `[@May1976]` | (May, 1976) | Parenthetical citation |
| `[-@May1976]` | \(1976\) | Suppress the author name |
| `[@May1976; @RN410]` | (May, 1976; Post and Forchhammer, 2002) | Multiple citations |
| `[@May1976, p. 461]` | (May, 1976, p. 461) | Citation with page number |

**Example in running text:**

``` markdown
@May1976 demonstrated that simple population models can produce complex chaotic dynamics. This finding has since been confirmed by
numerous studies [@RN410; @kamm2000].
```

### Changing the citation style

The citation formatting is controlled by the `.csl` file specified in
the YAML header. The default is SAGE Harvard style. To use a different
style:

1.  Find your desired style at <https://www.zotero.org/styles> or
    <https://github.com/citation-style-language/styles>

2.  Download the `.csl` file and place it in the `bib/` folder

3.  Update the YAML header:

    ``` yaml
    csl: bib/your-style.csl
    ```

### Reference managers

Using a reference manager saves significant time. Recommended options:

- **Zotero** (free, open source) — <https://www.zotero.org/>
  - Install the **Better BibTeX** plugin to automatically export and
    sync your library to a `.bib` file.
  - Zotero can import references directly from your browser with one
    click.
- **RStudio Visual Editor** — RStudio’s visual editing mode has a
  built-in citation tool. Switch to visual mode (click the “Visual”
  button at the top of the editor), then use **Insert \> Citation** to
  search and insert references from your `.bib` file, DOI lookup, or a
  connected Zotero library. This is very convenient for day-to-day
  writing.

### Citing R and packages (Software section)

At the end of your Material and Methods chapter, cite R and all packages
you used. The `packages.bib` file is automatically generated by the
`generate-package-refs` code chunk in `index.Rmd`. Use inline R code to
print version numbers dynamically:

``` markdown
All analyses were performed using the statistical software R (version 4.5.2)
[@R-base]. This thesis, including tables, was generated using the packages 'bookdown' (version 0.46)
[@R-bookdown], 'rmarkdown' (version 2.30) [@R-rmarkdown], 'knitr' (version 1.51)
[@R-knitr], and 'kableExtra' (version 1.4.0) [@R-kableExtra].
```

**Important:** When you add a new package to `index.Rmd` (in the
`load-packages` chunk), also add it to the
[`knitr::write_bib()`](https://rdrr.io/pkg/knitr/man/write_bib.html)
call in the `generate-package-refs` chunk so that its citation entry is
created automatically.

------------------------------------------------------------------------

## 7. Cross-References Summary

| Element | Syntax | Rendered example |
|:---|:---|:---|
| Figure | `\@ref(fig:chunk-label)` | Figure 3.1 |
| Table (kable/flextable) | `\@ref(tab:chunk-label)` | Table 2.1 |
| Table (R Markdown) | `\ref{tab:label}` | Table 1 |
| Equation | `\@ref(eq:label)` | Equation (1) |
| Section | `[Section name]` | clickable link to that section |

### Rules for labels

- Chunk labels must **not contain underscores**. Use hyphens:
  `scatter-fig` (correct), ~~`scatter_fig`~~ (will break
  cross-references).
- Figures and tables must have a caption to be numbered and
  cross-referenceable.
- For figures, the caption is set via the chunk option `fig.cap`.
- For kable tables, the caption is set via the `caption` argument in
  `kable()`.
- For flextable tables, the caption is set via `set_caption()`.
- For R Markdown tables, the caption is set with `Table: Caption text.`
  and the label with `\label{tab:name}`.

------------------------------------------------------------------------

## 8. Tips and Troubleshooting

### Version control

Use **Git** for your thesis from the very beginning. Commit early and
often. This protects you against accidental deletions and lets you see
the full history of your writing.

- Getting started with Git and RStudio: <https://happygitwithr.com/>

- Create a `.gitignore` that excludes generated files:

      thesis-output/
      _bookdown_files/
      cache/
      figures/
      *.log
      *.tex

### Common issues and solutions

| Problem | Solution |
|:---|:---|
| **“index.Rmd not found”** | Make sure you renamed `skeleton.Rmd` to `index.Rmd`. |
| **LaTeX package missing** | Run `tinytex::tlmgr_install("package-name")` or reinstall TinyTeX with [`tinytex::install_tinytex()`](https://rdrr.io/pkg/tinytex/man/install_tinytex.html). |
| **Encoding errors or garbled characters** | Ensure all `.Rmd` files are saved with UTF-8 encoding (File \> Save with Encoding \> UTF-8 in RStudio). |
| **Cross-references show “??”** | Check that the chunk label contains no underscores and that the figure/table has a caption. |
| **Bibliography not appearing** | Verify that all cited keys exist in your `.bib` file and that the file path in the YAML header is correct. |
| **Word output has formatting issues** | Some LaTeX commands (`\newpage`, `\vspace`, LaTeX tables) do not work in Word. Use Markdown or flextable alternatives. |
| **Figures appear on wrong page** | This is normal LaTeX behavior — it places floats where they fit best. Use cross-references so readers can find them. |
| **“Column ‘x’ doesn’t exist” in kable** | Make sure you load data from the `data/` folder with the correct relative path (e.g., `read.csv("data/mtcars.csv")`). |

### Keeping your project clean

- Store all data files in `data/`, all images in `images/`, and all
  bibliography files in `bib/`.
- Do not edit `template.tex` unless you know LaTeX well and need custom
  formatting.
- The `figures/` and `cache/` folders are generated automatically during
  knitting. You can safely delete them to force a clean rebuild.

### Adding or removing chapters

Edit `_bookdown.yml` to change the chapter order or add/remove files:

``` yaml
rmd_files: ["index.Rmd",
  "chapter/01-intro.Rmd",
  "chapter/02-methods.Rmd",
  "chapter/03-results.Rmd",
  "chapter/04-discussion.Rmd",
  "chapter/96-references.Rmd",
  "chapter/97-acknowledge.Rmd",
  "chapter/98-appendix.Rmd",
  "chapter/99-declaration.Rmd"
  ]
```

To add a new chapter, create a new `.Rmd` file in the `chapter/` folder
and add its path to the list above. The numbering prefix (e.g., `05-`)
controls the default sort order but the explicit list in `_bookdown.yml`
takes precedence.

### Removing optional sections

- **List of abbreviations:** Remove (or comment out) the
  `abbreviations:` lines in the YAML header of `index.Rmd`.
- **List of tables / List of figures:** Set `lot: false` and/or
  `lof: false` in the YAML header.
- **Appendix or Acknowledgements:** Remove the corresponding file from
  `_bookdown.yml`.

### Useful keyboard shortcuts in RStudio

| Action                         | Windows/Linux | macOS        |
|:-------------------------------|:--------------|:-------------|
| Knit document                  | Ctrl+Shift+K  | Cmd+Shift+K  |
| Insert code chunk              | Ctrl+Alt+I    | Cmd+Option+I |
| Comment out section            | Ctrl+Shift+C  | Cmd+Shift+C  |
| Save                           | Ctrl+S        | Cmd+S        |
| Visual Editor: Insert citation | Ctrl+Shift+F8 | Cmd+Shift+F8 |

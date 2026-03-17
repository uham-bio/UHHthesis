# Abschlussarbeit schreiben mit UHHthesis

Diese Referenz enthält alles, was du zum Schreiben deiner
Abschlussarbeit benötigst. Für Installation und Projekteinrichtung siehe
[Erste
Schritte](https://uham-bio.github.io/UHHthesis/articles/uhhthesis-getting-started-de.md).

> **Goldene Regel:** Knitte immer nur `index.Rmd`. Knitte niemals
> einzelne Kapiteldateien.

## 1. YAML-Konfiguration (index.Rmd)

Der YAML-Kopfbereich in `index.Rmd` steuert das Titelblatt, die
Formatierung und die Dokumenterstellung. Hier eine Übersicht der
wichtigsten Felder:

### Titelblatt

``` yaml
---
type: "Bachelorarbeit / Masterarbeit"   # Art der Arbeit
title: "Titel der Abschlussarbeit"      # Titel
author: "Mandy Mustermann"              # Autorin/Autor
birth: "31. Januar 1970"                # Geburtsdatum
degree_program: "Biologie"              # Studiengang
submit_date: "1. April 2000"            # Abgabedatum
advisor1: "Prof. A. Ersthelfer"         # 1. Gutachter/in
advisor2: "Prof. Z. Eswirdschonwerden"  # 2. Gutachter/in
---
```

> Ersetze alle Platzhalter durch deine eigenen Angaben!

### Vorangestellte Abschnitte (Prelim)

``` yaml
abstract: |
  
zusammenfassung: |
  
abbreviations: |
  
```

- Diese Zeilen lesen die jeweiligen Dateien aus `prelim/` automatisch
  ein.
- Wenn du **kein** Abkürzungsverzeichnis brauchst, lösche die beiden
  Zeilen für `abbreviations`.

### Verzeichnisse

``` yaml
lot: true    # Tabellenverzeichnis anzeigen (true/false)
lof: true    # Abbildungsverzeichnis anzeigen (true/false)
```

### Literatur

``` yaml
bibliography: [bib/references.bib, bib/packages.bib]
csl: bib/sage-harvard.csl
link-citations: true
```

- `references.bib` – deine manuell gepflegte Literaturdatenbank.
- `packages.bib` – wird automatisch durch den `generate-package-refs`
  Code-Chunk erzeugt.
- `sage-harvard.csl` – der Zitierstil. Kann durch eine andere .csl-Datei
  ersetzt werden.

### Formatierung

``` yaml
space_between_paragraphs: true
header-includes:
    - \usepackage{setspace}\onehalfspacing
```

- `space_between_paragraphs: true` fügt einen zusätzlichen Abstand
  zwischen Absätzen ein.
- `\onehalfspacing` setzt 1,5-fachen Zeilenabstand. Alternativen:
  `\singlespacing` oder `\doublespacing`.

### Ausgabeformat

``` yaml
knit: "bookdown::render_book"
output: UHHthesis::thesis_pdf_de
```

Diese Zeilen legen fest, dass bookdown zum Rendern verwendet wird und
die PDF-Ausgabe im deutschen Format erzeugt wird.

------------------------------------------------------------------------

## 2. R Markdown Grundlagen

R Markdown kombiniert normalen Text mit R-Code. Hier die wichtigsten
Formatierungen:

### Überschriften

``` markdown
# Kapitel (Ebene 1)
## Unterkapitel (Ebene 2)
### Abschnitt (Ebene 3)
```

Jede Kapiteldatei beginnt mit einer Überschrift der Ebene 1 (`#`).

### Unnummerierte Abschnitte

Manche Abschnitte wie Abstract oder Zusammenfassung sollen nicht
nummeriert werden. Das geht mit `{-}`:

``` markdown
# Zusammenfassung {-}
```

### Textformatierung

``` markdown
**Fetter Text**
*Kursiver Text*
~~Durchgestrichen~~
`Code inline`
```

### Listen

**Unnummeriert:**

``` markdown
- Erster Punkt
- Zweiter Punkt
  - Unterpunkt
```

**Nummeriert:**

``` markdown
1. Erster Punkt
2. Zweiter Punkt
   a. Unterpunkt
```

### Blockzitate

``` markdown
> Dies ist ein Blockzitat. Es wird eingerückt dargestellt.
```

### Fußnoten

``` markdown
Hier steht ein Satz mit Fußnote^[Das ist der Fußnotentext.].
```

### Kommentare (werden nicht im Dokument angezeigt)

``` markdown
<!-- Dies ist ein Kommentar, der im PDF nicht erscheint. -->
```

### Seitenumbruch

Im LaTeX/PDF-Format kannst du einen Seitenumbruch erzwingen:

``` markdown
\newpage
```

### Links

``` markdown
[Linktext](https://www.example.com)
```

### Inline-R-Code

R-Ausdrücke können direkt in den Fließtext eingebettet werden. Die
Syntax verwendet Backtick-r:

``` markdown
Der Datensatz enthält 32 Beobachtungen. Die mittlere Reichweite beträgt 20.1 Meilen pro Gallone.
```

Solche Inline-R-Ausdrücke (`` `r `` gefolgt vom R-Code und einem
schließenden Backtick) werden beim Knitten ausgewertet und durch das
Ergebnis ersetzt. Das ist besonders nützlich im Kapitel *Material und
Methodik*, um Software-Versionsnummern automatisch einzufügen (siehe
[Zitate und
Literaturverzeichnis](#id_10-zitate-und-literaturverzeichnis)).

### Code-Chunks

R-Code-Chunks werden mit dreifachen Backticks und `{r}` eingeleitet. Der
Chunk-Name und die Optionen stehen in den geschweiften Klammern:

```` default
```{r chunk-name, echo=FALSE, warning=FALSE}
# Dein R-Code hier
```
````

Hier eine Übersicht der wichtigsten Chunk-Optionen:

| Option | Beschreibung | Standard |
|:---|:---|:---|
| `echo` | R-Code in der Ausgabe anzeigen | `TRUE` |
| `eval` | Code ausführen | `TRUE` |
| `warning` | Warnungen anzeigen | `TRUE` |
| `message` | Meldungen anzeigen (z.B. von [`library()`](https://rdrr.io/r/base/library.html)) | `TRUE` |
| `include` | Chunk und Ausgabe einschließen | `TRUE` |
| `cache` | Ergebnisse zwischenspeichern (beschleunigt erneutes Knitten) | `FALSE` |
| `results` | Darstellung der Ergebnisse (`"asis"` für Rohausgabe) | `"markup"` |
| `fig.cap` | Abbildungsunterschrift (nötig für Nummerierung) | – |
| `fig.width` | Breite beim Erzeugen (Zoll) | `7` |
| `fig.height` | Höhe beim Erzeugen (Zoll) | `5` |
| `out.width` | Breite im Dokument | – |

Die Standardwerte für `echo`, `warning` und `message` werden global im
Setup-Chunk von `index.Rmd` gesetzt (über `knitr::opts_chunk$set()`),
sodass sie nicht in jedem Chunk wiederholt werden müssen.

Für abbildungs- und tabellenspezifische Optionen siehe
[Abbildungen](#id_7-abbildungen) und [Tabellen](#id_8-tabellen).

------------------------------------------------------------------------

## 3. Abbildungen

### Externe Bilder einbinden

Externe Bilddateien (z.B. JPG, PNG) werden mit
[`knitr::include_graphics()`](https://rdrr.io/pkg/knitr/man/include_graphics.html)
in einem R-Code-Chunk eingebunden. Die Abbildungsbeschriftung wird über
die Chunk-Option `fig.cap` festgelegt, die Breite über `out.width`:

```` default
```{r location, echo=FALSE, fig.cap="Lage des Untersuchungsstandorts.", out.width="50%"}
knitr::include_graphics("images/sampling_site.jpg")
```
````

- `location` ist das **Label** des Chunks (wichtig für Querverweise).
- Die Bilddatei muss im angegebenen Pfad relativ zum Projektordner
  liegen.

### R-generierte Abbildungen

Abbildungen können direkt mit R-Code erstellt werden. Hier ein Beispiel
mit Base-R:

```` default
```{r base-fig, fig.cap="Beziehung zwischen Pferdestärke und Reichweite.", out.width="100%"}
plot(mtcars$hp, mtcars$mpg,
  xlab = "Pferdestärke",
  ylab = "Reichweite (Meilen pro Gallone)",
  pch = 19)
abline(lm(mpg ~ hp, data = mtcars), col = "red")
```
````

### Beispiel mit ggplot2

```` default
```{r scatter-fig, fig.cap="Streudiagramm: Hubraum vs. Reichweite, farblich nach Zylinderanzahl.", out.width="80%", fig.width=6, fig.height=4}
library(ggplot2)
ggplot(mtcars, aes(x = disp, y = mpg, colour = factor(cyl))) +
  geom_point(size = 2) +
  geom_smooth(method = "lm", se = FALSE) +
  labs(
    x = "Hubraum (cu.in.)",
    y = "Reichweite (Meilen/Gallone)",
    colour = "Zylinder"
  ) +
  theme_minimal()
```
````

### Boxplot-Beispiel

```` default
```{r boxplot-fig, fig.cap="Reichweite nach Getriebetyp (0 = Automatik, 1 = Manuell).", out.width="50%", fig.width=5, fig.height=3, fig.align="center"}
boxplot(mpg ~ am, data = mtcars,
  xlab = "Getriebetyp",
  ylab = "Reichweite (Meilen/Gallone)",
  col = c("lightblue", "salmon"))
```
````

### Wichtige Chunk-Optionen für Abbildungen

| Option       | Beschreibung                | Beispiel               |
|:-------------|:----------------------------|:-----------------------|
| `fig.cap`    | Abbildungsunterschrift      | `fig.cap="Mein Plot."` |
| `out.width`  | Breite im Dokument          | `out.width="80%"`      |
| `fig.width`  | Breite beim Erzeugen (Zoll) | `fig.width=6`          |
| `fig.height` | Höhe beim Erzeugen (Zoll)   | `fig.height=4`         |
| `fig.align`  | Ausrichtung                 | `fig.align="center"`   |
| `echo`       | Code anzeigen?              | `echo=FALSE`           |
| `dpi`        | Auflösung                   | `dpi=300`              |

### Querverweise auf Abbildungen

Abbildungen lassen sich referenzieren, wenn der Code-Chunk einen
**Namen** (Label) und eine **`fig.cap`** hat. Die Syntax für
Querverweise:

``` markdown
Siehe Abbildung \@ref(fig:scatter-fig).
```

- `fig:` gibt an, dass es sich um eine Abbildung handelt.
- `scatter-fig` ist der Name des Code-Chunks.

> **Wichtig:** Chunk-Namen dürfen **keine Unterstriche** (`_`)
> enthalten! Verwende stattdessen Bindestriche (`-`).

------------------------------------------------------------------------

## 4. Tabellen

### R Markdown Tabellen (manuell)

Einfache Tabellen können direkt in Markdown geschrieben werden:

``` markdown
Table: Dies ist eine manuell geschriebene Tabelle. \label{tab:rmd-tab}

| Spalte A     | Spalte B         | Spalte C       |
|:-------------|:----------------:|---------------:|
| linksbündig  | zentriert        | rechtsbündig   |
| 123          | 456              | 789            |
| *kursiv*     | normal           | **fett**       |
```

- Die Doppelpunkte in der Trennzeile bestimmen die Ausrichtung.
- `\label{tab:rmd-tab}` setzt das Label für Querverweise
  (LaTeX-Notation).

**Querverweis auf Markdown-Tabellen** (LaTeX-Notation!):

``` markdown
Siehe Tabelle \ref{tab:rmd-tab}.
```

> **Hinweis:** Markdown-Tabellen verwenden die LaTeX-Notation
> (`\label{}` und `\ref{}`) anstelle der bookdown-Notation (`\@ref()`).
> Das liegt daran, dass bookdown das Label nur bei
> Code-Chunk-generierten Tabellen automatisch erkennt.

### kableExtra (empfohlen für PDF)

Das Paket `kableExtra` baut auf
[`knitr::kable()`](https://rdrr.io/pkg/knitr/man/kable.html) auf und
bietet viele Formatierungsmöglichkeiten für LaTeX/PDF-Tabellen:

```` default
```{r kable1}
library(kableExtra)
mtcars_sub <- read.csv("data/mtcars.csv")
df <- mtcars_sub[1:5, 1:6]

knitr::kable(df,
  booktabs = TRUE,
  caption = "Beispieltabelle mit knitr::kable() und kableExtra."
) |>
  kable_styling(position = "center", font_size = 9) |>
  add_header_above(c(" " = 1, "Gruppe 1" = 2, "Gruppe 2" = 2, "Gruppe 3" = 1)) |>
  footnote(general = "Hier können Fußnoten stehen.")
```
````

- Die Tabellenbeschriftung wird über das Argument `caption` in
  [`knitr::kable()`](https://rdrr.io/pkg/knitr/man/kable.html)
  festgelegt.
- Das Label für Querverweise ist automatisch der **Chunk-Name** (hier:
  `kable1`).

**Querverweis:**

``` markdown
Siehe Tabelle \@ref(tab:kable1).
```

> **Hinweis:** `kableExtra` funktioniert hervorragend für die
> PDF-Ausgabe, ist aber **nicht** mit der Word-Ausgabe kompatibel. Für
> Word-Dokumente verwende `flextable`.

### flextable (funktioniert für PDF und Word)

Das Paket `flextable` ist formatunabhängig und funktioniert sowohl für
PDF als auch für Word:

```` default
```{r flex-tab}
library(flextable)
df <- mtcars[1:5, 1:4]

ft <- flextable(df) |>
  theme_booktabs() |>
  autofit() |>
  set_caption(
    caption = "Beispieltabelle mit flextable.",
    autonum = officer::run_autonum(seq_id = "tab", bkm = "flex-tab")
  )
ft
```
````

- `set_caption()` mit `autonum` erzeugt eine nummerierte
  Tabellenbeschriftung.
- Das Argument `bkm` legt das Lesezeichen (Label) für den Querverweis
  fest.

**Querverweis:**

``` markdown
Siehe Tabelle \@ref(tab:flex-tab).
```

> **Tipp:** Wenn du sowohl PDF als auch Word erzeugen möchtest, ist
> `flextable` die sicherste Wahl, da es für beide Formate funktioniert.

### Hinweis zum gt-Paket

Das Paket [gt](https://gt.rstudio.com/) ist eine moderne Alternative für
Tabellen in R. Die PDF-Unterstützung über LaTeX ist allerdings noch
nicht ganz ausgereift. Für reine HTML-Ausgaben ist `gt` sehr
empfehlenswert. Für die Thesis-PDF-Vorlage empfiehlt sich derzeit eher
`kableExtra` oder `flextable`.

### Welches Paket soll ich verwenden?

| Paket | PDF | Word | Gleicher Code für beides? | Am besten für |
|:---|:--:|:--:|:--:|:---|
| Markdown | ✓ | ✓ | ✓ | Kleine, manuell geschriebene Tabellen |
| kableExtra | ✓ | ✗ | – | PDF-Ausgabe, viele Online-Beispiele |
| **flextable** | **✓** | **✓** | **✓** | **Beste Wahl für PDF und Word** |
| gt | eingeschränkt | ✓ | meistens | Moderne API, vielfältige Formatierung |

**Empfehlung:** Wenn du nur PDF erzeugst, ist **kableExtra** eine gute
Wahl mit umfangreicher Dokumentation. Wenn du sowohl PDF als auch Word
erzeugen möchtest, nimm **flextable** — es liefert in beiden Formaten
konsistente Ergebnisse ohne zusätzlichen Code.

### Querverweise auf Tabellen – Zusammenfassung

| Tabellen-Typ | Label setzen | Querverweis |
|:---|:---|:---|
| R Markdown (manuell) | `\label{tab:name}` | `\ref{tab:name}` |
| [`knitr::kable()`](https://rdrr.io/pkg/knitr/man/kable.html) / `kableExtra` | automatisch = Chunk-Name | `\@ref(tab:chunk-name)` |
| `flextable` | über `bkm` in `set_caption()` | `\@ref(tab:bkm-name)` |

------------------------------------------------------------------------

## 5. Mathematische Gleichungen und chemische Formeln

### Mathematische Gleichungen

#### Inline-Modus

Gleichungen innerhalb des Textflusses werden zwischen einfachen
Dollarzeichen geschrieben:

``` markdown
Die berühmte Formel $E = mc^2$ von Einstein...
```

> **Wichtig:** Zwischen `$` und der Formel darf **kein Leerzeichen**
> stehen!

#### Display-Modus (zentriert, eigene Zeile)

Gleichungen, die in einer eigenen Zeile stehen sollen, erhalten doppelte
Dollarzeichen:

``` markdown
$$E = mc^2$$
```

#### Nummerierte Gleichungen mit Querverweisen

Für nummerierte Gleichungen wird die LaTeX-Umgebung `\begin{equation}`
verwendet. Das Label wird mit `(\#eq:label)` vergeben:

``` markdown
\begin{equation}
  \bar{X} = \frac{\sum_{i=1}^n X_i}{n}
  (\#eq:mean)
\end{equation}
```

**Querverweis:**

``` markdown
Siehe Gleichung \@ref(eq:mean).
```

#### Weitere Beispiele

Dichtefunktion der Standardnormalverteilung:

``` markdown
\begin{equation}
  f_{Y}(y) = \varphi(y) \stackrel{\mathrm{def}}{=}
  \frac{1}{\sqrt{2 \pi}} \exp \left\{ -\frac{y^2}{2} \right\},
  \quad y \in \mathbb{R}.
  (\#eq:density-norm)
\end{equation}
```

> **Hinweis:** Gleichungen sollten immer in Sätze integriert werden und
> entsprechend mit Komma oder Punkt enden.

### Chemische Formeln

Chemische Formeln werden am besten in LaTeX-Notation geschrieben. Da der
Mathematikmodus in LaTeX standardmäßig kursiv setzt, Formeln aber
aufrecht stehen sollten, wird `\mathrm{}` verwendet:

#### Grundlagen

``` markdown
- Summenformel: $\mathrm{Fe_2^{2+}Cr_2O_4}$
- Hochgestellt (Ladung): $\mathrm{O^-}$
- Tiefgestellt: $\mathrm{CH_4}$
- Gestapelt (tief + hoch): $\mathrm{Fe_2^{2+}}$
```

#### Symbole für Reaktionsgleichungen

``` markdown
- Reaktionspfeil: $\longrightarrow$
- Gleichgewichtspfeil: $\rightleftharpoons$
- Resonanzpfeil: $\leftrightarrow$
- Delta: $\Delta$
```

#### Reaktionsgleichungen

Reaktionen können als nummerierte Gleichungen geschrieben werden:

``` markdown
\begin{equation}
  \mathrm{C_6H_{12}O_6 + 6O_2} \longrightarrow \mathrm{6CO_2 + 6H_2O}
  (\#eq:reaction)
\end{equation}
```

Inline-Beispiel:

``` markdown
$\mathrm{NH_4Cl_{(s)}} \rightleftharpoons \mathrm{NH_{3(g)} + HCl_{(g)}}$
```

------------------------------------------------------------------------

## 6. Zitate und Literaturverzeichnis

### Wie es funktioniert

Die UHHthesis-Vorlage nutzt **BibTeX** zur Verwaltung der
Literaturquellen. Alle Referenzen stehen in `bib/references.bib`, und
Pandocs eingebauter Zitierprozessor (**citeproc**) formatiert sie
automatisch gemäß dem Stil in der YAML-Konfiguration (`.csl`-Datei).

**Wo erscheint das Literaturverzeichnis?** Die Datei
`chapter/96-referenzen.Rmd` bestimmt, wo die Referenzliste im Dokument
platziert wird. Du musst in dieser Datei **nichts** schreiben — sie
funktioniert automatisch. Das entscheidende Element darin ist ein
spezieller HTML-Tag:

``` html
<div id="refs"></div>
```

Dieser sagt Pandoc: “Füge die erzeugte Literaturliste **hier** ein.”
Ohne diesen Tag würde das Verzeichnis ganz am Ende des Dokuments
erscheinen (nach dem Anhang und der Eigenständigkeitserklärung). Die
Datei enthält außerdem LaTeX-Befehle für den hängenden Einzug in der
PDF-Ausgabe. **Verändere oder lösche diese Datei nicht.**

### BibTeX-Format

Alle Literaturquellen werden in der Datei `bib/references.bib` im
BibTeX-Format gespeichert. Ein Eintrag sieht z.B. so aus:

``` bibtex
@article{May1976,
  author  = {May, R. M.},
  title   = {Simple mathematical models with very complicated dynamics},
  journal = {Nature},
  volume  = {261},
  number  = {5560},
  pages   = {459--467},
  year    = {1976},
  DOI     = {10.1038/261459a0}
}
```

Häufige Eintragstypen:

| Typ              | Verwendung                            |
|:-----------------|:--------------------------------------|
| `@article`       | Zeitschriftenartikel                  |
| `@book`          | Buch                                  |
| `@incollection`  | Buchkapitel                           |
| `@inproceedings` | Konferenzbeitrag                      |
| `@phdthesis`     | Dissertation                          |
| `@mastersthesis` | Masterarbeit                          |
| `@techreport`    | Technischer Bericht                   |
| `@manual`        | Software-Handbuch                     |
| `@misc`          | Sonstiges (Webseiten, Software, etc.) |

### Zitationssyntax

In R Markdown wird mit dem `@`-Zeichen und dem BibTeX-Schlüssel zitiert:

| Syntax               | Ergebnis (Beispiel SAGE Harvard)      |
|:---------------------|:--------------------------------------|
| `@May1976`           | May (1976)                            |
| `[@May1976]`         | (May, 1976)                           |
| `[-@May1976]`        | \(1976\) – nur Jahreszahl             |
| `[@May1976; @RN410]` | (May, 1976; Post & Forchhammer, 2002) |
| `@May1976 [S. 460]`  | May (1976, S. 460)                    |

Beispiel im Fließtext:

``` markdown
@May1976 konnte zeigen, dass einfache Populationsmodelle komplexe chaotische Dynamiken auslösen können. Diese Ergebnisse wurden
später von anderen Autoren bestätigt [@RN410].
```

### Literaturverwaltung

Es empfiehlt sich, von Anfang an ein Literaturverwaltungsprogramm zu
verwenden:

- **[Zotero](https://www.zotero.org/)** (kostenlos, Open Source) – sehr
  empfohlen!
  - Kann direkt .bib-Dateien exportieren (über das Plugin [Better
    BibTeX](https://retorque.re/zotero-better-bibtex/))
  - Integration mit dem RStudio Visual Editor (s.u.)
- **[Mendeley](https://www.mendeley.com/)** – ebenfalls geeignet

#### RStudio Visual Editor

Der RStudio Visual Editor (oben rechts im Editor auf “Visual”
umschalten) bietet eine grafische Oberfläche zum Einfügen von Zitaten:

1.  Klicke auf **Insert \> Citation…** (oder Strg+Shift+F8 /
    Cmd+Shift+F8).
2.  Suche nach Referenzen aus Zotero, DOI, Crossref oder deiner
    .bib-Datei.
3.  Die Referenz wird automatisch in den Text und in die .bib-Datei
    eingefügt.

### Zitierstile (CSL)

Der Stil des Literaturverzeichnisses wird über eine `.csl`-Datei
gesteuert. Die Vorlage nutzt standardmäßig den SAGE-Harvard-Stil. Du
kannst andere Stile verwenden:

1.  Suche den gewünschten Stil unter <https://www.zotero.org/styles>
    oder im
    [GitHub-Repository](https://github.com/citation-style-language/styles).
2.  Lade die `.csl`-Datei herunter und lege sie im Ordner `bib/` ab.
3.  Ändere den Pfad im YAML-Kopfbereich:

``` yaml
csl: bib/mein-anderer-stil.csl
```

### Software zitieren

Am Ende des Kapitels *Material und Methodik* sollten die verwendete
Software und R-Pakete mit Versionsnummern aufgeführt werden. Die Vorlage
enthält dafür bereits einen Abschnitt. Der Code-Chunk
`generate-package-refs` in `index.Rmd` erstellt automatisch die Datei
`bib/packages.bib` mit den Referenzen aller geladenen Pakete.

Beispieltext:

``` markdown
Die Analysen wurden mit der Statistiksoftware R (Version
4.5.2)
[@R-base] durchgeführt. Die Abschlussarbeit wurde mit den Paketen
'bookdown' (Version 0.46) [@R-bookdown]
und 'knitr' (Version 1.51) [@R-knitr] erstellt.
```

> **Wichtig:** Stelle sicher, dass alle Pakete, die du im
> `load-packages`-Chunk in `index.Rmd` lädst, auch im
> `generate-package-refs`-Chunk aufgelistet sind!

------------------------------------------------------------------------

## 7. Querverweise – Zusammenfassung

Hier eine Übersicht aller Querverweis-Typen:

| Ziel | Syntax | Beispiel |
|:---|:---|:---|
| **Abbildung** (Code-Chunk) | `\@ref(fig:chunk-name)` | `Abbildung \@ref(fig:scatter-fig)` |
| **Tabelle** (kable/flextable) | `\@ref(tab:chunk-name)` | `Tabelle \@ref(tab:kable1)` |
| **Tabelle** (Markdown) | `\ref{tab:label}` | `Tabelle \ref{tab:rmd-tab}` |
| **Gleichung** | `\@ref(eq:label)` | `Gleichung \@ref(eq:mean)` |
| **Kapitel/Abschnitt** | `[Kapitelname]` | `[Diskussion]` |

### Regeln für Querverweise

1.  **Chunk-Namen** dürfen keine Unterstriche enthalten – verwende
    Bindestriche.
2.  Abbildungs-Querverweise funktionieren nur, wenn **sowohl** ein
    Chunk-Name **als auch** `fig.cap` gesetzt sind.
3.  Die automatische Nummerierung in `_bookdown.yml` sorgt dafür, dass
    Abbildungen als “Abbildung X.Y”, Tabellen als “Tabelle X.Y” und
    Gleichungen als “Gleichung X” beschriftet werden (konfigurierbar
    unter `language > label`).
4.  Für Kapitel-Links innerhalb des Dokuments genügen eckige Klammern um
    den exakten Kapitelnamen, z.B. `[Material und Methodik]`.

------------------------------------------------------------------------

## 8. Tipps und Fehlerbehebung

### Versionskontrolle mit Git

Es ist sehr empfehlenswert, deine Arbeit mit Git zu versionieren:

``` bash
cd deine-arbeit/
git init
git add .
git commit -m "Initiales Thesis-Projekt"
```

Füge eine `.gitignore`-Datei hinzu, die generierte Dateien ausschließt:

    thesis-output/
    _bookdown_files/
    cache/
    figures/
    *.log
    *.tex
    *.synctex.gz

### Häufige Probleme und Lösungen

| Problem | Lösung |
|:---|:---|
| `skeleton.Rmd` statt `index.Rmd` | Datei in `index.Rmd` umbenennen |
| Einzelne Kapiteldatei geknitted | Immer nur `index.Rmd` knitten! |
| LaTeX-Paket fehlt | `tinytex::tlmgr_install("paketname")` ausführen |
| “Error: Failed to compile … .tex” | Log-Datei in `thesis-output/` prüfen (`thesis.log`) |
| Querverweis zeigt `??` oder `@ref(...)` | Chunk-Name prüfen (keine Unterstriche!), `fig.cap` gesetzt? |
| Tabelle erscheint nicht / Label fehlt | Bei `kable()`: `caption`-Argument gesetzt? Bei Markdown: `\label{}` vorhanden? |
| Abbildung wird an falscher Stelle platziert | Das ist normales LaTeX-Verhalten (“Floating”). Nicht versuchen, es zu erzwingen – LaTeX optimiert die Platzierung. |
| Cache-Probleme (alte Ergebnisse) | Ordner `cache/` und `_bookdown_files/` löschen und neu rendern |
| Encoding-Probleme mit Umlauten | Sicherstellen, dass alle Dateien in **UTF-8** gespeichert sind (RStudio: File \> Save with Encoding \> UTF-8) |
| PDF wird nicht aktualisiert | Alte PDF-Datei im Viewer/Reader schließen, `thesis-output/` leeren, neu rendern |
| Word-Tabellen werden nicht korrekt dargestellt | `flextable` statt `kableExtra` verwenden |

### Projekt sauber halten

- Speichere alle Datendateien in `data/`, alle Bilder in `images/` und
  alle Bibliographiedateien in `bib/`.
- Bearbeite `template.tex`nur, wenn du dich gut mit LaTeX auskennst und
  individuelle Formatierungen benötigst.
- Die Ordner `figures/` und `cache/` werden beim Knitten automatisch
  erzeugt. Du kannst sie bedenkenlos löschen, um einen sauberen
  Neuaufbau zu erzwingen.

### Kapitel hinzufügen oder entfernen

Bearbeite `_bookdown.yml`, um die Kapitelreihenfolge zu ändern oder
Dateien hinzuzufügen bzw. zu entfernen:

``` yaml
rmd_files: ["index.Rmd",
  "chapter/01-einleitung.Rmd",
  "chapter/02-methodik.Rmd",
  "chapter/03-ergebnisse.Rmd",
  "chapter/04-diskussion.Rmd",
  "chapter/96-referenzen.Rmd",
  "chapter/97-danksagung.Rmd",
  "chapter/98-anhang.Rmd",
  "chapter/99-versicherung.Rmd"
  ]
```

Um ein neues Kapitel hinzuzufügen, erstelle eine neue `.Rmd`-Datei im
Ordner `chapter/` und füge den Dateipfad zur obigen Liste hinzu. Das
Nummernpräfix (z.B. `05-`) bestimmt die Standard-Sortierreihenfolge,
aber die explizite Liste in `_bookdown.yml` hat Vorrang.

### Optionale Abschnitte entfernen

- **Abkürzungsverzeichnis**: Entferne (oder kommentiere aus) die
  `abbreviations:`-Zeilen im YAML-Header von `index.Rmd`.
- **Tabellenverzeichnis / Abbildungsverzeichnis**: Setze `lot: false`
  und/oder `lof: false` im YAML-Header.
- **Anhang oder Danksagung**: Entferne die entsprechende Datei aus
  `_bookdown.yml`.

### Nützliche Tastenkürzel in RStudio

| Aktion                        | Windows/Linux | macOS        |
|:------------------------------|:--------------|:-------------|
| Dokument knitten              | Strg+Shift+K  | Cmd+Shift+K  |
| Code-Chunk einfügen           | Strg+Alt+I    | Cmd+Option+I |
| Abschnitt auskommentieren     | Strg+Shift+C  | Cmd+Shift+C  |
| Speichern                     | Strg+S        | Cmd+S        |
| Visual Editor: Zitat einfügen | Strg+Shift+F8 | Cmd+Shift+F8 |

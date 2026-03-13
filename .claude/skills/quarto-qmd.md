---
name: quarto-qmd
description: Quarto Markdown (.qmd) reference -- consult when creating, editing, or debugging any .qmd file or Quarto project configuration.
---

# Quarto Markdown (.qmd) Reference

## Overview

Quarto is an open-source scientific publishing system. QMD files are Markdown
files with YAML frontmatter and executable code cells that compile to HTML,
PDF, Word, slides, dashboards, and more.

## File Structure

A `.qmd` file has three sections:

1. **YAML frontmatter** (delimited by `---`)
2. **Markdown body** (standard + Quarto extensions)
3. **Code cells** (delimited by `` ```{language} `` / `` ``` ``)

### YAML Frontmatter Essentials

- `title`, `author`, `date` -- metadata
- `format` -- output format (`html`, `pdf`, `docx`, `revealjs`, `dashboard`)
- `execute` -- code execution options (`echo`, `eval`, `warning`, `cache`)
- `bibliography` -- path to `.bib` file
- `csl` -- citation style

### Code Cell Options

Cell-level options use `#|` prefix inside the cell:

- `#| label: fig-example` -- cross-referenceable label
- `#| echo: false` -- hide source code in output
- `#| eval: true` -- execute the cell
- `#| fig-cap: "Caption"` -- figure caption
- `#| tbl-cap: "Caption"` -- table caption
- `#| output: asis` -- raw output (useful for dynamic Markdown)
- `#| cache: true` -- cache cell results

### Supported Languages

- Python (`{python}`)
- R (`{r}`)
- Julia (`{julia}`)
- Observable JS (`{ojs}`)
- Mermaid diagrams (`{mermaid}`)
- Dot/Graphviz (`{dot}`)

## Cross-References

- Figures: `@fig-label`
- Tables: `@tbl-label`
- Sections: `@sec-label`
- Equations: `@eq-label`
- Listings: `@lst-label`

Labels must be set in the code cell with `#| label: fig-xxx` or on sections
with `{#sec-xxx}`.

## Quarto Project Structure

Multi-document projects use `_quarto.yml`:

- `project: type: website|book|manuscript`
- `website: title, navbar, sidebar`
- `book: chapters, appendices`

## Common Gotchas

- YAML frontmatter indentation matters -- use 2 spaces, no tabs
- Cross-reference labels must start with `fig-`, `tbl-`, `sec-`, `eq-`, `lst-`
- `execute: freeze: auto` is recommended for projects with expensive computations
- PDF output requires a LaTeX distribution (TinyTeX: `quarto install tinytex`)
- On Windows, use `quarto render` not `quarto preview` in CI scripts
- Callout blocks use `:::{.callout-note}` / `:::` syntax (fenced div)
- Tabsets use `::: {.panel-tabset}` / `:::`
- Include files with `{{< include file.qmd >}}`

## What NOT to Do

- Do not use raw HTML for structure that Quarto provides natively (callouts, tabsets, columns)
- Do not hardcode figure/table numbers -- use cross-references
- Do not mix R Markdown chunk syntax (`{r, echo=FALSE}`) with Quarto syntax (`#| echo: false`)
- Do not put executable code in `.md` files -- use `.qmd`

## Testing / Verification

- `quarto check` -- verify installation and dependencies
- `quarto render file.qmd` -- render a single file
- `quarto preview` -- live preview with hot reload (dev only, not CI)
- Check rendered output for broken cross-references (shows as `?ref`)

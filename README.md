# Dynamic Year

Dynamic Year is a Quarto shortcode extension for keeping years and dates
relative to a project-level `base-year`.

It provides:

- `{{< dynamic-year offset >}}` for year-only values;
- `{{< dynamic-date "YYYY-MM-DD" offset >}}` for dates where only the year
  changes;
- `dynamic-year.R` helpers for using the same logic in R chunks.

## Installation

In a Quarto project, copy this folder to:

```text
_extensions/dynamic-year/
```

Then define a reference year in `_quarto.yml` or in a document front matter:

```yaml
base-year: 2026
```

## Codex User Skill

This repository includes a simple Codex skill for authors who want help using
the extension:

```text
.agents/skills/dynamic-year-user/
```

When Codex is started inside this repository, the skill is discovered
automatically because Codex scans `.agents/skills` from the working directory up
to the repository root. Invoke it explicitly with:

```text
$dynamic-year-user
```

To install the user skill for personal use across repositories, copy the skill
folder to the current user skills location:

```bash
mkdir -p ~/.agents/skills
cp -R .agents/skills/dynamic-year-user ~/.agents/skills/
```

Codex usually detects skill changes automatically. If the skill does not appear
in the skill selector or via `$dynamic-year-user`, restart Codex.

## Quarto Shortcodes

Use `dynamic-year` with an integer offset:

```markdown
Current year: {{< dynamic-year 0 >}}
Previous year: {{< dynamic-year -1 >}}
```

Use `dynamic-date` with `YYYY` as the year placeholder:

```markdown
Start date: {{< dynamic-date "YYYY-10-15" >}}
Previous cohort: {{< dynamic-date "YYYY-10-15" -1 >}}
```

By default, impossible dates raise an error. To silence that error and adjust
the date, set `invalid`:

```markdown
{{< dynamic-date "YYYY-02-30" invalid="previous-month-end" >}}
{{< dynamic-date "YYYY-02-30" invalid="next-month-start" >}}
```

## R Helpers

Source the R helpers from a chunk:

```r
source("_extensions/dynamic-year/dynamic-year.R")

dynamic_year()
dynamic_year(-1)
dynamic_date("YYYY-10-15")
dynamic_date("YYYY-02-30", invalid = "previous-month-end")
```

`dynamic_base_year()` reads `base-year` from the current document front matter
or from the nearest `_quarto.yml`.

## Example

The rendered example is available at <https://alpa12.github.io/dynamic-year/>.

Render the bundled example:

```bash
quarto render example.qmd --to html
```

When this folder is used as a standalone GitHub repository, the included GitHub
Actions workflow renders `example.qmd` and publishes it to GitHub Pages on every
push to `main`.

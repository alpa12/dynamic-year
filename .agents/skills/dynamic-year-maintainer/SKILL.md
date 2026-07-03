---
name: dynamic-year-maintainer
description: Maintain and evolve the Dynamic Year Quarto extension in this repository. Use when Codex is asked to modify, debug, release, document, test, or review the extension files `dynamic-year.lua`, `dynamic-year.R`, `_extension.yml`, `README.md`, `example.qmd`, generated examples, or project-specific agent skills.
---

# Dynamic Year Maintainer

## Project Map

Maintain a small Quarto shortcode extension:

- `_extension.yml`: extension metadata, Quarto requirement, contributed shortcode file.
- `dynamic-year.lua`: Quarto shortcode implementation for `{{< dynamic-year >}}` and `{{< dynamic-date >}}`.
- `dynamic-year.R`: R helpers that mirror the shortcode behavior inside executable chunks.
- `README.md`: user-facing installation and usage documentation.
- `example.qmd`: canonical executable example; keep it aligned with the README and rendered output.
- `.agents/skills/`: agent-facing operating knowledge for maintainers and users.

## Behavioral Contract

Preserve these public behaviors unless the user explicitly asks for a breaking change:

- Require `base-year` in document front matter or nearest `_quarto.yml`.
- Treat `base-year` and offsets as integers only.
- Render `{{< dynamic-year >}}` as `base-year`; render `{{< dynamic-year -1 >}}` as `base-year - 1`.
- Render `{{< dynamic-date "YYYY-MM-DD" >}}` by replacing only the year and preserving month/day.
- Accept an optional integer offset as the second positional argument to `dynamic-date`.
- Reject invalid months and day values below 1.
- For impossible dates such as February 30, default to an error.
- Support `invalid="previous-month-end"` and `invalid="next-month-start"` in Lua shortcodes.
- Support `invalid = "previous-month-end"` and `invalid = "next-month-start"` in R helpers.
- Keep leap-year logic consistent between Lua and R.
- Keep R helpers able to process `Date`, numeric, and character inputs, including `YYYY` and `YYYY-MM-DD` strings.

## Maintenance Workflow

Start by reading the relevant source and documentation before editing. For behavior changes, inspect both `dynamic-year.lua` and `dynamic-year.R`; the two implementations should stay intentionally parallel.

When changing behavior:

1. Update the Lua shortcode implementation.
2. Update the R helper implementation to match.
3. Update `README.md` with the user-facing contract.
4. Update `example.qmd` with at least one canonical example when the behavior is visible to users.
5. Update these project skills when their guidance becomes stale.
6. Render or test the example before committing.
7. Commit the completed change immediately with a focused message.

Always commit changes as work progresses. Prefer small, coherent commits after each verified functional/documentation step instead of accumulating unrelated edits.

## Validation

Use the narrowest validation that proves the change:

```bash
quarto render example.qmd --to html
```

For R-only edits, also run a small `Rscript --vanilla -e` check that sources `dynamic-year.R` and exercises `dynamic_year()`, `dynamic_date("YYYY")`, a normal date, an invalid date repair, and a leap-year edge case.

For shortcode parsing changes, include at least one rendered `example.qmd` case using Quarto's escaped shortcode syntax for the displayed source and a live shortcode for the result.

## Documentation Rules

Keep `README.md`, `example.qmd`, and this skill in sync with the implemented behavior. If a user-facing option, error mode, argument type, or installation step changes, update all three in the same commit.

Keep examples short and executable. Prefer one canonical example per feature over exhaustive prose.

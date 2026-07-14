# GENESIS Tutorial 2026 — Style Guide

This guide covers cross-chapter writing conventions for the `genesis_website`
repository. For chapter structure and required metadata fields, see the
chapter template in `_drafts/`. For file and directory layout in
`genesis_tutorial_materials`, see `CONTRIBUTING.md` in that repository.

---

## 1. Voice and tone

- Write in the second person: "Let's move to...", "Now run GENESIS...",
  "You should see...".
- Keep sentences short. If a sentence needs more than two commas, split it.
- Explain *why* before *how*: describe the purpose of a step in one
  sentence before showing the commands.
- Do not re-explain concepts already covered in earlier chapters.
  Cross-reference instead (see §8).

---

## 2. Front matter

Every tutorial page requires the following YAML front matter:

```yaml
---
title: "GENESIS Tutorial X.Y (2026)"
gpos: 00X.00Y
excerpt: "One sentence description."
last_modified_at: 2026-XX-XXT00:00:00+09:00
layout: single
toc: true
toc_sticky: true
sidebar:
  nav: sidebar-basic
---
```

`gpos` numbering controls the order of Previous/Next navigation buttons
and must follow this format exactly:

- Normal chapters: `{:03d}.{:03d}` — e.g. Tutorial 3.2 → `003.002`,
  Tutorial 11.1 → `011.001`
- Appendices: `999.{:03d}` — e.g. Appendix 5 → `999.005`

Do not modify any other front matter fields.

---

## 3. Section headings

- The H1 heading (`#`) is the full chapter title, written as a sentence
  — do **not** include the chapter number prefix:
  - ❌ `# 11.1 Coarse-grained simulation of protein with AICG2+ model`
  - ✅ `# Coarse-grained simulation of protein with AICG2+ model`
- Use H2 (`##`) for main steps and H3 (`###`) for sub-steps.
- The on-page TOC is generated automatically from headings; do not write
  a manual TOC.

---

## 4. Code blocks

Three types of code block are used in tutorials:

**Shell commands** — use `bash`, prefix every command with `$`:

```bash
$ cd 01_setup
$ ls
01_oripdb  02_psfgen
```

**GENESIS control files** — use `toml`, add an inline `#` comment on
every non-obvious parameter, and align `=` signs within each section:

```toml
[DYNAMICS]
integrator    = VVER     # velocity Verlet integrator
nsteps        = 500000   # number of MD steps
timestep      = 0.002    # timestep (ps)
eneout_period = 500      # energy output period
```

**Program output and log excerpts** — no language tag, truncate with `:`
lines if the output is long (see §5):
```
:  
INFO: STEP TIME TOTAL_ENE  
INFO: 0 0.0000 -6.0756  
:
```

Additional rules for all code blocks:
- Do not leave unnecessary blank lines at the end of a code block.
- Inline code must be enclosed in backticks. Remove any surrounding
  double quotes: `"`some-code`"` → `` `some-code` ``
- Remove any legacy `{...}` style attributes that affect rendering,
  such as `{.underline}` or `{.gsc_oci_title_link ...}`.

---

## 5. Truncating long output

Never paste more than ~15 lines of raw log or terminal output. Use `:`
on its own line to indicate omitted content:
```
:  
[first few relevant lines]  
:  
[last few relevant lines]  
:
```

Always explain in the surrounding text which columns or fields the
reader should look at.

---

## 6. Math equations

Use LaTeX syntax rendered by MathJax:

- **Inline equations**: `\\( ... \\)`  
  e.g. `\\( E = mc^2 \\)`
- **Block equations**: `\\[ ... \\]`, or `\\[ \begin{aligned} ... \end{aligned} \\]`
  for multi-line aligned equations.

If an equation does not render correctly, try adding a space at the end
of each line inside the equation block.

For superscripts and subscripts in **non-math prose**, use HTML tags:
- Superscript: `<sup>3</sup>` → ³
- Subscript: `<sub>3</sub>` → ₃

Do not use `^` or `_` for super/subscripts outside of math blocks.

---

## 7. Images and figures

Basic syntax:

```markdown
![Caption text](/assets/images/filename.png)
```

- The path **must** start with `/`. A missing leading slash is a common
  cause of broken images.
- To resize: `![](/assets/images/filename.png){: width="400"}`
- To resize and center-align:
  `![](/assets/images/filename.png){: width="200" .align-center}`
- Preferred format: **PNG**, ≥ 150 dpi.
- VMD screenshots should use a white or black background, not the
  default grey.

Every figure must have a caption that states what the reader should
**observe**, not just what the figure shows:
- ❌ "Time course of the potential energy."
- ✅ "Potential energy as a function of time. The energy fluctuates
     around −1.3 kcal/mol after ~100 ps, indicating equilibration."

**Embedding video/animations:**

```html
<video width="480" controls="controls" style="display: block; margin: 0 auto;">
  <source src="/assets/images/filename.mp4" type="video/mp4">
  Brief description of the video (optional fallback text).
</video>
```

Do not use raw HTML `<img>` tags; use standard Markdown image syntax instead.

---

## 8. Cross-references and links

Use inline Markdown links. Always link to the chapter title, not a bare
number:

- ✅ `as described in [Tutorial 3.2](/tutorials/genesis_tutorial_3.2_2026/)`
- ❌ `as described in Tutorial 3.2`
- ❌ `see above`

Check that all internal and external links resolve correctly before
opening a PR. The CI link-checker (GitHub Actions + lychee) will also
catch broken links automatically on every PR.

---

## 9. Notice boxes

Use Minimal Mistakes notice classes for important callouts:

```markdown
This is a plain notice.
{: .notice}

**Important**: This is an info-styled notice.
{: .notice--info}
```

Available classes: `notice`, `notice--info`, `notice--warning`,
`notice--danger`, `notice--success`.

Use `notice--info` for prerequisite reminders and version requirements.
Use `notice--warning` for common pitfalls. Do not overuse — limit to
situations where the information would be easy to miss in body text.

---

## 10. Citations and references

Use Markdown footnote syntax throughout:

- In-text citation: `[^1]`, `[^2]`, ...
- Multiple consecutive citations:
  `[^2]<sup>,</sup>[^3]` → renders as ²˒³
- Citation placed after a punctuation mark:
  `apple,[^2] and banana.[^3]`

Reference definitions go at the end of the file, before the author line:

```markdown
[^1]: Li, W., Wang, W., Takada, S., **2014**, *Proc. Natl. Acad. Sci.*,
      111, 10550–10555.
[^2]: Clementi, C., Nymeyer, H. & Onuchic, J. N., **2000**,
      *Journal of Molecular Biology*, 298, 937–953.
```

---

## 11. Analysis toolchain

**Shell one-liners** (`awk`, `grep`, `tail`) are acceptable for simple
log parsing on the login node — quick sanity checks before a full Python
environment is loaded:

```bash
$ grep "INFO:" log | awk '{print $3, $5}' > energy.dat
```

**Python is the primary analysis tool** for anything beyond a quick
check: plotting, statistical analysis, RMSD/RMSF, and any figure that
appears in the tutorial. Show a minimal, self-contained script in full:

```python
import numpy as np
import matplotlib.pyplot as plt

data = np.loadtxt("energy.dat")
plt.plot(data[:, 0], data[:, 1])
plt.xlabel("Time (ps)")
plt.ylabel("Potential energy (kcal/mol)")
plt.tight_layout()
plt.savefig("energy.png", dpi=150)
```

Pin all Python dependencies in `requirements.txt` at the root of
`genesis_tutorial_materials`. Do not use packages outside this list
without adding them there first.

Do not use Jupyter notebooks. Plain `.py` scripts are reproducible,
diffable, and runnable on HPC login nodes without a browser.

---

## 12. File paths

- Use **relative paths** inside code blocks wherever possible.
  Absolute paths such as `/home/user/GENESIS/bin/atdyn` are fragile —
  use the symlinked `./bin/atdyn` instead.
- In prose, refer to files and directories with backticks:
  `INP`, `log`, `01_setup/`, `toppar/`.
- Do not hardcode usernames or home directories in example commands.

---

## 13. Expected results

Every analysis section must tell the reader what a correct result looks
like. Provide at least one of:

- A specific numerical range: "The averaged potential energy should be
  approximately −1.3 kcal/mol."
- A qualitative description of the expected plot shape.
- A pointer to the reference output:
  "Compare your output with `04_analysis/reference/energy.dat`."

---

## 14. Author attribution

Every chapter ends with an attribution block in italics, after a
horizontal rule, outside any section heading. Use the `{: .notice}`
class so it renders as a styled box:

```markdown
---
*Written by [Full Name] @ [Institution], [Month Year]*
{: .notice}
```

When updating an existing chapter, add a new line rather than replacing
the original:

```markdown
---
*Written by [Name A] @ [Institution], [Month Year]*  
*Updated by [Name B] @ [Institution], [Month Year]*
{: .notice}
```

---

## 15. Local preview before opening a PR

Always render the site locally before submitting changes:

```bash
$ bundle exec jekyll serve
```

Then open `http://127.0.0.1:4000` in a browser and navigate to your
chapter to verify:

- The page title is correct and contains no leading chapter number.
- All images are displayed (broken images show as empty boxes).
- All code blocks have correct syntax highlighting.
- All math equations render correctly.
- All internal and external links resolve.
- Notice boxes render with the correct style.
- The Previous/Next navigation buttons point to the correct chapters.

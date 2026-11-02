# Cardiac ischaemia dataset — parameter reference guide

**Live page:** https://ekrdzo.github.io/cardiac-ischemia-reference/

A single self-contained HTML file that documents the twelve columns of a 918-record cardiac
ischaemia dataset: what each column is defined to hold, how it is coded, what it separates in
this cohort, and where each external claim came from. No build step, no network calls, no
dependencies — the file carries its own data, styles and scripts, so it works offline and can
be emailed as an attachment. Saved locally it stays fully functional forever.

## Citation discipline

Every external claim on the page carries a numbered reference. Each identifier (PMID, PMCID or
DOI) was resolved by lookup and the supporting passage read before the claim was kept; the
reference list prints what each source is being used for. A claim that could not be resolved
that way is **not** quietly dropped or softened — it prints as an endnote with the reason it
fell short, and section 11 lists the figures and claims that were withdrawn during preparation
instead of reworded. Figures computed from the embedded records need no citation and reproduce
from the data in the file.

## Data

918 records embedded in the page. Column definitions follow the **UCI Heart Disease field
documentation** (Steinbrunn W., Detrano R., Janosi A., et al.; P. Ryan, repository editor),
which the page quotes directly. Note what the target column actually records: angiographic
disease status, defined in the source's own words as *"> 50% diameter narrowing"* in a major
vessel. That is a lumen measurement, not confirmed ischaemia, and the page says so where it
matters for interpretation.

## This repository

Holds the **built output only**, as `index.html`. The page is generated from source parts in a
course working tree and is never hand-edited, so the copy here is replaced wholesale rather than
patched — edit the parts, rebuild, republish.

## Scope

Written as a reference guide for a university module. It is teaching material about a dataset,
not clinical guidance, not a diagnostic tool, and not validated for any decision about a
patient. If you are reading it because something in it looks actionable: it isn't.

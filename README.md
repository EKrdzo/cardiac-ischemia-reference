# Cardiac ischaemia dataset — parameter reference guide

**Live page:** https://ekrdzo.github.io/cardiac-ischemia-reference/

A single self-contained HTML file that documents the twelve columns of a 918-record cardiac
ischaemia dataset: what each column is defined to hold, how it is coded, what it separates in
this cohort, where each external claim came from — and, in section 9, the small neural network
trained on these same records, running in the browser on a patient the reader types in. No build
step, no network calls, no dependencies: the file carries its own data, model weights, styles and
scripts, so it works offline and can be emailed as an attachment. Saved locally it stays fully
functional forever.

## The trained model (section 9)

Section 9 carries the artefact produced by the companion PyTorch pipeline — 3,905 weights, the
preprocessing constants, the 643-row imputation donor pool, and the probability PyTorch assigned to
every one of the 918 embedded records. A reader enters a patient and the entire pipeline re-runs
locally: pandas-style parsing of the file's four spellings of "no measurement", sentinel masking of
impossible values, Z-scoring, reconstruction of the missing cells from the five nearest training
patients, the missingness indicators, then the forward pass with BatchNorm in its eval form. Any
field may be left blank. Nothing is transmitted and nothing is looked up.

That section publishes its own verification rather than asking to be believed. On load it re-scores
all 918 records in JavaScript, compares them with the stored PyTorch probabilities, and prints a
pass/fail badge with the largest difference before any output is shown — currently 0 disagreements
on 918 records and 5 hand-built inputs, largest difference 6.9e-07 against a tolerance of 1e-4. Held
out (275 records) it scores 0.8945 accuracy, 0.8930 balanced accuracy, 0.9049 F1, 0.9480 ROC-AUC,
with 138/15/108/14 TP/FP/TN/FN, and prints those beside three baselines fitted on the identical
matrix — majority class, logistic regression, random forest — where a linear model proves
competitive. The page draws that conclusion rather than shielding the network from it.

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

That applies with more force to the predictor than to the rest of the page. Section 9 reproduces a
trained network faithfully — that is precisely what its badge measures — and what it reproduces is a
model that learned from a few hundred patients of one historical research cohort, whose target
column records a lumen measurement rather than confirmed ischaemia, and whose accuracy a plain
logistic regression matches. A probability coming back from that form is the artefact behaving as
it was fitted on that cohort. It is not a risk estimate, and not a statement about whoever is
typing.

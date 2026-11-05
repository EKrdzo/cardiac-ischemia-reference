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
on 918 records and 5 hand-built inputs, largest difference 6.9e-07 against a tolerance of 1e-4.

**The accuracy figure to quote is 0.8693 (95 % CI 0.8475–0.8900), with a rank-AUC of 0.9325
(0.9163–0.9476)** — measured over all 918 records by three repeats of stratified five-fold
cross-validation, 15 fits, in which every record is predicted only by models that never trained on
it and the imputer, the scaler and the imputation donor pool are fitted inside each fold's own
training rows. The interval resamples records; the spread across repeats is a separate question and
both are printed rather than collapsed into one too-narrow band.

The single 275-record split that the shipped artefact was stopped on — 0.8945 accuracy, 0.8930
balanced accuracy, 0.9049 F1, 0.9480 ROC-AUC, 138/15/108/14 TP/FP/TN/FN — is printed beside those
figures rather than instead of them, because the gap between the two is the interesting part. That
split chose the epoch at which training stopped, so it is scored on a decision it took part in: an
arm that applies the same stopping rule inside the training fold only, where the scored rows can
influence nothing, lands 0.84 points lower (*t* = −3.15, 14 df, paired across the same 15 fits). The
run also stores a threshold sweep reaching 0.9164 at a cut of 0.56, which is the best of 91
thresholds found on the rows it then reports on and is therefore unusable; choosing the cut on
training rows moves it to 0.39, within a few hundredths of the default on F1 — the decision point is
not where the interest lies. And the same configuration retrained with the seed, the split and every
setting left exactly as published, on a different library build (torch 2.15.0 on Windows against
2.11.0 on Linux), stops at epoch 34 rather than 13 and scores 0.8982 — three true positives'
difference, same code, same data, same seed.

Three baselines fitted on the identical matrix — majority class, logistic regression, random forest —
are printed alongside, where a linear model proves competitive. The page draws that conclusion rather
than shielding the network from it, and it quotes the cross-validated interval rather than the
favourable split, on the grounds that only one of the two survives being asked a different question.

## Getting around it

A contents bar sits under the masthead and stays there while you scroll, marking which section you
are in. Its twelve links are the twelve section headings — it is a list of anchors, not a new way of
organising the page — and it measures its own height so a jump lands below it rather than behind it.

Any **dotted label carries a definition**: the twelve columns in the section 9 form, the feature chips
and comparison-table rows in section 4, the category headings in section 5, the column headers of the
record table in section 8, and the two outcome keys in every legend. Forty-three labels in all. The
sentence inside each box is the one section 2's dictionary prints for that column, read out of the same
data structure at hover time rather than retyped, which is what stops a tooltip from going stale
against the table it explains or smuggling in a clinical claim that no source was checked against. The
underline only appears where a dictionary entry actually exists, and definitions open on keyboard focus
as well as on hover, so nothing on the page requires a mouse.

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

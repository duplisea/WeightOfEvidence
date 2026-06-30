## Background

Biological reference points — such as *B*<sub>*M**S**Y*</sub>, the Upper
Stock Reference (USR), and the Limit Reference Point (LRP) — are
typically estimated assuming that the productivity of a fish stock is
stationary over time. However, fish stocks can undergo persistent shifts
in productivity driven by ecosystem change, climate, or demographic
effects that are not attributable to fishing pressure alone. When such
shifts occur, continuing to manage against reference points calibrated
to a prior productivity regime may be inappropriate, potentially setting
harvest levels that are too high for a stock now operating at lower
productivity, or unnecessarily restrictive for a stock that has shifted
to higher productivity.

Determining *when* such a shift has occurred — and when it is
sufficiently well-evidenced to justify revising reference points — is a
critical and contested question in fisheries management. DFO is
developing a formal process for evaluating time-varying reference points
(TVRPs), including explicit criteria for when productivity change is
sufficiently documented to warrant a reference point revision. This tool
is part of that process.

The core concern, following **Klaer et al. (2015)**, is that reference
point revisions should not be made on weak or incomplete evidence, and
should not be used as a mechanism to accommodate chronically low stock
status caused by ongoing overfishing. A structured, transparent
weight-of-evidence framework protects against ad hoc or politically
motivated revisions.

------------------------------------------------------------------------

## What This Tool Does

This single-file HTML application implements a structured
**weight-of-evidence scoring framework** for evaluating whether a fish
stock has undergone a productivity shift sufficient to justify revising
its biological reference points. It operationalises the framework of
Klaer et al. (2015), extended with:

- Formal IPCC-style confidence integration (evidence quality ×
  agreement)
- A third evidence domain for Indigenous Knowledge (IK) and Fisher
  Knowledge (FK)
- Four sequential prerequisites (reversibility, F-reduction test,
  motivation audit, management capability)
- Sensitivity analysis on scoring assumptions
- An evidence timeline visualisation
- CSV import/export for reproducibility and audit trails

The tool is entirely self-contained — a single `.html` file with no
external dependencies, no server, and no data transmission. It runs in
any modern browser.

------------------------------------------------------------------------

## The Scoring Framework

Evidence is organised into three domains:

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 37%" />
<col style="width: 29%" />
</colgroup>
<thead>
<tr>
<th>Domain</th>
<th>Content</th>
<th>Notes</th>
</tr>
</thead>
<tbody>
<tr>
<td><strong>A — Internal Stock Signal</strong></td>
<td>Fecundity, natural mortality, failed recovery, recruitment,
maturity, skip spawning, weight-at-age, length-at-age, age truncation,
spatial contraction</td>
<td>3 lines are Critical importance (<span
class="math inline"><em>w</em><sub><em>I</em></sub> = 1.0</span>)</td>
</tr>
<tr>
<td><strong>B — Ecosystem &amp; Environment</strong></td>
<td>Coherent species shifts, prey/predator, temperature, habitat,
dissolved oxygen, zooplankton</td>
<td>Most meaningful when coherent with Domain A</td>
</tr>
<tr>
<td><strong>C — IK &amp; FK</strong></td>
<td>Indigenous knowledge and fisher knowledge (stock and ecosystem
observations)</td>
<td>Treated as full independent evidence streams</td>
</tr>
</tbody>
</table>

### Weighted Score per Evidence Line

Each evidence line *i* receives a weighted score formed by multiplying
four components:

*W*<sub>*i*</sub> = *S*<sub>*i*</sub> × *C*<sub>*i*</sub> × *Q*<sub>*i*</sub> × *I*<sub>*i*</sub>

<table>
<colgroup>
<col style="width: 29%" />
<col style="width: 40%" />
<col style="width: 29%" />
</colgroup>
<thead>
<tr>
<th>Symbol</th>
<th>Parameter</th>
<th>Levels</th>
</tr>
</thead>
<tbody>
<tr>
<td><span
class="math inline"><em>S</em><sub><em>i</em></sub></span></td>
<td>Evidence score</td>
<td>0 — absent, 1 — weak, 2 — moderate, 3 — strong</td>
</tr>
<tr>
<td><span
class="math inline"><em>C</em><sub><em>i</em></sub></span></td>
<td>Confidence weight (IPCC-derived)</td>
<td>0.25, 0.50, 0.75, or 1.00</td>
</tr>
<tr>
<td><span
class="math inline"><em>Q</em><sub><em>i</em></sub></span></td>
<td>Data quality weight</td>
<td>Poor = 0.50, Fair = 0.67, Good = 0.83, Excellent = 1.00</td>
</tr>
<tr>
<td><span
class="math inline"><em>I</em><sub><em>i</em></sub></span></td>
<td>Importance weight</td>
<td>Supporting = 0.25, Moderate = 0.50, High = 0.75, Critical =
1.00</td>
</tr>
</tbody>
</table>

The confidence weight *C*<sub>*i*</sub> is derived automatically from
the IPCC two-axis framework (evidence quality × agreement level) and is
not entered directly by the user.

### Total Score and Framework Maximum

The total weighted score sums over all scored lines across all domains:

Total = ∑<sub>*i* ∈ scored</sub>*W*<sub>*i*</sub> × *d*<sub>*k*</sub>

where *d*<sub>*k*</sub> is the user-adjustable domain weight for domain
*k* ∈ {*A*, *B*, *C*} (default *d*<sub>*k*</sub> = 1.0 for all domains).

The **full framework maximum** is computed over all lines not explicitly
marked Not Applicable (N/A):

Maximum = ∑<sub>*i* ∉ N/A</sub>3 × 1.00 × 1.00 × *I*<sub>*i*</sub> × *d*<sub>*k*</sub>

That is, the maximum assumes every non-N/A line scores 3 (strong
evidence), Excellent data quality, and High agreement.

### Score Percentage

$$\text{Score} = \frac{\text{Total}}{\text{Maximum}} \times 100$$

The denominator is **fixed at the full framework maximum**, not only the
subset of lines the user chooses to assess. A stock assessed on only
three lines will score much lower than one assessed comprehensively,
even if each individual line scores perfectly. This makes scores
directly comparable across stocks and assessments, and follows the
spirit of Klaer et al. (2015) in treating unevaluated criteria as
scoring zero.

### Reference Benchmarks

Scores are interpreted relative to two empirical reference cases:

<table>
<colgroup>
<col style="width: 44%" />
<col style="width: 19%" />
<col style="width: 36%" />
</colgroup>
<thead>
<tr>
<th>Reference case</th>
<th>Score</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td><strong>Northern Cod (2J3KL)</strong></td>
<td>~72%</td>
<td>All 10 Domain A lines assessed at strong scores; 5 of 7 Domain B
lines assessed. Well-documented multi-indicator shift. Represents a
strong evidence case.</td>
</tr>
<tr>
<td><strong>Hypothetical weak evidence</strong></td>
<td>~18%</td>
<td>Only 3 of 22 lines assessed at low scores. All remaining lines score
0 against the full denominator. Represents an insufficient evidence
case.</td>
</tr>
</tbody>
</table>

Klaer et al. (2015) found that a score of approximately 7/12 (~58%) was
empirically associated with acceptance of a productivity shift in
peer-reviewed assessments. The equivalent threshold in this framework is
approximately **35–55%** of the full framework maximum, depending on
domain coverage.

------------------------------------------------------------------------

## How to Use

1.  **Download** `tvrp_tool.html` and open it in any modern web browser
    (Chrome, Firefox, Edge, Safari). No installation or internet
    connection required.

2.  **Phase 0:** Enter the stock name, mean generation time, and primary
    assessment approach. Select the reason(s) a productivity shift is
    being considered. Adjust domain weights if there is a documented
    rationale (default: equal weights, *d*<sub>*k*</sub> = 1.0).

3.  **Domain A:** For each of the 10 internal stock signal lines,
    either:

    - Click **Assess this line** to open the scoring fields and assign
      an evidence score (0–3), data quality, agreement level, direction
      of change, onset year, and notes; or
    - Click **Not applicable to this stock** to exclude the line from
      the denominator.
    - Lines left as *Not yet assessed* score 0 and are included in the
      denominator.

4.  **Domain B:** Same procedure for the 7 ecosystem and environmental
    evidence lines. For ocean temperature, note the species’ thermal
    preference in the notes field, as the direction valence is
    context-dependent.

5.  **Domain C:** Same procedure for the 5 IK & FK lines. Document
    consultation status (IK and FK separately). Flag any lines where IK
    or FK contradicts scientific evidence — reconciliation notes are
    required.

6.  **Prerequisites:** Complete the four sequential prerequisite
    evaluations:

    - *Prerequisite 1:* Reversibility and life history changes
    - *Prerequisite 2:* F-reduction test (was F reduced sufficiently and
      for long enough?)
    - *Prerequisite 3:* Motivation audit (internal use only — does not
      appear in exported output)
    - *Prerequisite 4:* Management system capability (is the
      contemplated reference point change larger than management
      precision?)

7.  **Summary:** Review the score positioning chart, domain
    contributions, evidence findings, prerequisite outcomes, and
    evidence gaps. Edit synthesis statements for each domain and an
    overall synthesis. Export the full evaluation as a CSV for the
    assessment record.

### Import / Export

- **Export CSV:** Saves all scores, metadata, prerequisite evaluations,
  and synthesis statements to a `.csv` file named
  `[stockname]_tvrp_[date].csv`.
- **Import CSV:** Re-loads a previously exported file to resume or
  review an evaluation.

------------------------------------------------------------------------

## Important Caveats

- This tool **does not replace** peer review, IK/FK consultation
  processes, or management advisory processes.
- All scores reflect the **professional judgment** of the assessment
  team. The tool structures and records that judgment; it does not
  automate it.
- A high score is a necessary but not sufficient condition for a
  reference point revision. Prerequisite evaluations — particularly the
  reversibility and F-reduction tests — must also be satisfied.
- The tool is designed to be used **within** a formal DFO stock
  assessment or CSAS process, not as a standalone decision instrument.

------------------------------------------------------------------------

## References

Duplisea, D.E., Eddy, T.D., Robertson, M.D., Ruiz-Díaz, R., Solberg,
C.A. & Zhang, F. (2025). The ghosts of overfishing past that haunt
present day fisheries management. *Canadian Journal of Fisheries and
Aquatic Sciences*, 82(1), 1–14.

Hutchings, J.A. (2000). Collapse and recovery of marine fishes.
*Nature*, 406(6798), 882–885.

Hutchings, J.A. & Reynolds, J.D. (2004). Marine fish population
collapses: Consequences for recovery and extinction risk. *BioScience*,
54(4), 297–309.

IPCC (2010). *Guidance note for lead authors of the IPCC fifth
assessment report on consistent treatment of uncertainties*.
Intergovernmental Panel on Climate Change.

Klaer, N.L., O’Boyle, R.N., Deroba, J.J., Wayte, S.E., Little, L.R.,
Alade, L.A. & Rago, P.J. (2015). How much evidence is required for
acceptance of productivity regime shifts in fish stock assessments: Are
we letting managers off the hook? *Fisheries Research*, 168, 49–55.

Vert-Pre, K.A., Amoroso, R.O., Jensen, O.P. & Hilborn, R. (2013).
Frequency and intensity of productivity regime shifts in marine fish
stocks. *Proceedings of the National Academy of Sciences*, 110(5),
1779–1784.

------------------------------------------------------------------------

## Acknowledgements

The application logic, scoring framework, and interface were developed
with substantial coding assistance from **Claude** (Anthropic) and
**Google Gemini**. The framework design, domain structure, evidence line
selection, prerequisite criteria, and calibration to the Northern Cod
reference case reflect the professional judgment of the authors and draw
directly on the published literature cited above.

------------------------------------------------------------------------

## Licence

This tool is provided for use within DFO assessment and advisory
processes. Please contact the authors before adapting or redistributing.

# Patient Retention Analysis: Homoeopathic Clinic (In Progress)

> **Status:** Data collection phase. No results are published yet. This README describes the question, design and method, and will be updated as each stage is completed.
**Last updated:** November 2026. [Schema finalised; digitising case records is next.]

## Summary

A small homoeopathic clinic in Goa, India (run by a family member) keeps all patient records on paper. This project builds an anonymised dataset from those records and uses cohort and retention analysis to answer a practical question for the clinic: **which new patients come back, and which do not?**

## Problem

The clinic's lead doctor has noticed that patients who miss their first follow-up (due about 15 days after the first visit) rarely return. There is no data on how common this is, who it affects, or how long returning patients stay engaged. This project puts numbers on both.

## Questions

1. **Initial dropout:** what share of new patients attend their first follow-up?
2. **Secondary retention:** of the patients who do attend, how long do they keep returning before they lapse?
3. **Segments:** do these rates differ by complaint category, age bracket, gender, region or consultation mode (in-person vs call/video)?

## Key definitions

| Term | Definition used in this project |
|---|---|
| Local patient | Resident in Goa or elsewhere in India, excluding patients based abroad. Patients based abroad (who return roughly once a year) are excluded because their visit pattern is different. |
| Initial dropout | New patient who does not attend a first follow-up within **30 days** of the first visit (expected at about 15 days, plus a grace period) |
| Lapsed | Patient with no visit for **45 days** after their last visit (cutoff agreed with the clinic's lead doctor) |

These follow a distinction made in outpatient-retention research between **initial dropout** (missing the first follow-up) and **secondary dropout** (disengaging later).

## Data

**Source:** paper case records, digitised by hand into a spreadsheet.

**Sample:** a random sample of about 100-150 local patients whose first visit fell between January and June 2024, followed through all of their later visits.

**Design:** two linked tables.

`patients` (one row per patient)

| Field | Notes |
|---|---|
| `patient_id` | Anonymous sequential ID (P001, P002, ...) |
| `age_bracket` | Brackets, not exact age |
| `gender` | As recorded on the case paper (F, M, Other). Missing values kept as "Not recorded" |
| `area_region` | North Goa, South Goa or Elsewhere in India. Broad region only, derived from the address and never stored as a village or town. Missing values kept as "Not recorded" |
| `first_visit_month` | Month and year only (e.g. 2024-03). Groups patients into cohorts |
| `observation_days` | Days from first visit to end of data collection (derived) |

`visits` (one row per visit)

| Field | Notes |
|---|---|
| `visit_id` | Unique ID |
| `patient_id` | Links to `patients` |
| `days_since_first_visit` | 0 for the first visit. Used instead of calendar dates |
| `visit_type` | New or follow-up (first follow-up flagged) |
| `complaint_category` | Broad category chosen from a fixed dropdown list |
| `consult_mode` | In-person, call or video |

Exact dates are held in a private working file outside this repository, and only the relative fields above are used for analysis and publication.

Gender and region are entered for every patient; where a case paper does not state them, the value is "Not recorded" and the share of missing values will be reported. Referral source is not collected because it is not consistently recorded.

## Privacy and ethics

- **Anonymised at entry:** no names, phone numbers or addresses are entered. Each patient gets a coded ID, and the ID-to-patient key is kept on paper at the clinic, not in this repository or on any device used for analysis.
- **Broad categories only:** complaints are recorded as broad categories, ages as brackets, and visit timing as days since the first visit rather than calendar dates.
- **No patient-level data in this repository.** Only aggregated results, charts and synthetic example rows are published.
- **Permission:** The clinic’s lead practitioner has approved the use of anonymised records for this project.
- Follows the professional-secrecy duties in the National Commission for Homoeopathy's Code of Ethics (2022).

## Method (planned)

- [ ] Digitise the sampled case records into the two-table structure
- [ ] Clean and quality-check (duplicates, visit-day calculations, inconsistent categories)
- [ ] Explore the data (distributions, visit gaps)
- [ ] Calculate the initial dropout rate
- [ ] Build cohort tables and retention curves (45-day lapse rule)
- [ ] Compare segments (complaint category, age bracket, gender, region, consultation mode)
- [ ] Write up findings and recommendations for the clinic

## Tools

Excel (data entry with validated dropdowns), Python (Pandas), SQL. Power BI dashboard in progress.

## Background

- Research on dropout from outpatient care separates *initial* dropout from *secondary* dropout. See Henzen et al., "Determinants of dropout in a community-based mental health crisis centre", BMC Psychiatry (2016): https://pmc.ncbi.nlm.nih.gov/articles/PMC4837516/
- In outpatient research, retention can be defined by repeat visits within a set time window. For example, Fleishman et al. (2012) counted a patient as retained if they had at least two outpatient visits 90 days apart in each year in care ([J Acquir Immune Defic Syndr 60(3):249-259](https://pmc.ncbi.nlm.nih.gov/articles/PMC3383913/)). This project instead uses a clinic-specific lapse cutoff of 45 days, agreed with the clinic's lead doctor, because homoeopathic follow-up intervals vary with the medicine supplied.

## Anticipated limitations

- A small sample (about 100-150 patients), so subgroup comparisons will rest on small numbers and will be treated as descriptive, not conclusive.
- Data entered by hand from paper records, with some ambiguity in handwriting and notes.
- A single clinic, so findings may not generalise.
- Gender and region may be missing for some patients (shown as ‘Not recorded’).

## Repository structure

```
.
├── README.md
├── requirements.txt
├── docs/
│   ├── data_dictionary.md
│   └── decisions_log.md
├── notebooks/        # analysis notebooks
├── visuals/          # exported charts
└── data/
    ├── README.md
    └── sample/       # synthetic example rows only
```

## Decisions and changes

Changes to the questions, sample or definitions are logged with dates and reasons in [`docs/decisions_log.md`](docs/decisions_log.md).

## Results

*Coming soon.*

## Author

Tanushri Borkar. MSc Data and Decision Analytics, University of Southampton.

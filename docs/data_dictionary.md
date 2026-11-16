# Data dictionary

Exact dates are kept in a private working file outside this repository. Only relative time and month-level fields are used for analysis and publication.

## patients (one row per patient)
| Field | Type | Description |
|---|---|---|
| patient_id | text | Anonymous sequential ID |
| age_bracket | text | e.g. 30-39 |
| gender | text | F / M / Other, as recorded on the case paper. "Not recorded" if missing |
| area_region | text | North Goa / South Goa / Elsewhere in India. Derived from the address, never the address itself. "Not recorded" if missing |
| first_visit_month | text | Month and year of the first (new case) visit, e.g. 2024-03 |
| observation_days | integer | Days from the first visit to the end of data collection (derived) |

## visits (one row per visit)
| Field | Type | Description |
|---|---|---|
| visit_id | text | Unique visit ID |
| patient_id | text | Links to patients |
| days_since_first_visit | integer | 0 for the first visit (derived from exact dates in the private file) |
| visit_type | text | New / Follow-up |
| complaint_category | text | Broad category from a fixed list |
| consult_mode | text | In-person / Call / Video |

## Complaint categories (fixed dropdown list)
Respiratory, Digestive, Skin, Musculoskeletal, Chronic-General, Mental-Emotional, Pediatric, Other

## Derived fields (to be built)
- days_since_previous_visit
- attended_first_followup (yes/no)
- lapsed (no visit within 45 days of last visit, judged against observation_days)

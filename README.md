# France Company 360 by SIREN: contracts, BODACC, labels

[![Apify Actor](https://img.shields.io/badge/Apify-Actor-1f6feb)](https://apify.com/phonic_lily/france-company-360-siren) ![Licence](https://img.shields.io/badge/licence-Licence_Ouverte_2.0-blue) ![Records](https://img.shields.io/badge/records-7%2C508-green)

One card per French company (SIREN) crossing public contracts won, BODACC notices, official food-safety inspections and RGE, EPV and supplier labels. Legal entities only, at least two sources each. Embedded data, pay per row.

## What you get
- **7,508 records**, each carrying its own `license` and `attribution` fields, ready to republish with the required credit.
- A **100-record free sample** in this repository: [`sample.jsonl`](sample.jsonl) and [`sample.csv`](sample.csv) (opens as a table on GitHub).
- The **full dataset** as a filterable API (JSON, CSV, Excel), no scraping, no login: [France Company 360 by SIREN: contracts, BODACC, labels on Apify](https://apify.com/phonic_lily/france-company-360-siren).

## One record
```json
{
  "siren": "027250174",
  "name": "DESRIAC SOCIETE",
  "naf": "43.22A",
  "legal_form_code": "5710",
  "legal_form": "SAS",
  "creation_date": "1966-01-01",
  "active": true,
  "city": null,
  "postal_code": null,
  "department": null,
  "workforce_band_code": null,
  "sources_count": 3
}
```

## Quick start
Python:
```python
from apify_client import ApifyClient
client = ApifyClient('<YOUR_APIFY_TOKEN>')
run = client.actor('phonic_lily/france-company-360-siren').call(run_input={'maxResults': 10})
for item in client.dataset(run['defaultDatasetId']).iterate_items():
    print(item)
```
Or from the command line:
```bash
curl -X POST 'https://api.apify.com/v2/acts/phonic_lily~france-company-360-siren/run-sync-get-dataset-items?token=<YOUR_APIFY_TOKEN>' \
  -H 'Content-Type: application/json' -d '{"maxResults": 10}'
```
Pricing is pay-per-record on Apify (see the Actor page); the sample here is free.

## Fields
| Field | Description |
|---|---|
| `siren` | Company SIREN (9 digits). Legal entities only. |
| `name` | Legal name from the INSEE SIRENE extract. |
| `naf` | NAF/APE activity code, e.g. 56.30Z. |
| `legal_form_code` | INSEE legal-form code (never 1xxx: no individual entrepreneurs). |
| `legal_form` | Legal form label (SAS, SARL...). |
| `creation_date` | Creation date of the legal unit (ISO). |
| `active` | True if the legal unit was administratively active in the SIRENE extract. |
| `city` | City, where the SIRENE extract carried it, else null. |
| `postal_code` | Postal code, where available, else null. |
| `department` | Department code, where available, else null. |
| `workforce_band_code` | INSEE workforce band code, null if unknown. |
| `sources_count` | Number of source families that know this SIREN among bodacc, decp, alimconfiance, rge, epv, rfar (always >= 2). |
| `sources_list` | Space-separated source families present: bodacc decp alimconfiance rge epv rfar. |
| `contracts_won_count` | Public contracts won (DECP, distinct contracts). Null if the SIREN is not a DECP supplier. |
| `contracts_total_declared_amount_eur` | Sum of usable declared contract amounts in EUR; null if none, never 0. Not a measure of revenue. |
| `contracts_distinct_buyers_count` | Distinct public buyers (SIRET) in DECP. |
| `contracts_last_notification_date` | Latest DECP notification date (ISO). |
| `bodacc_notices_count` | BODACC notices about this SIREN in our extracts (business purchases + registrations + strike-offs). Null if none. |
| `bodacc_business_purchases_count` | BODACC business-sale notices where the company is the buyer (2023-07 to 2026-07). |
| `bodacc_registration_notices_count` | BODACC registration (creation) notices (2025-08 to 2026-07). |
| `bodacc_strike_off_notices_count` | BODACC strike-off (radiation) notices (2025-08 to 2026-07). |
| `bodacc_last_notice_date` | Date of the latest BODACC notice among those counted (ISO). |
| `bodacc_last_notice_kind` | Kind of that latest notice: business-sale, creation or radiation. |
| `food_inspections_count` | Official food-safety inspections (Alim'confiance) over all establishments of the company in the published window (about 12 months). Null if none. |
| `food_establishments_inspected_count` | Distinct establishments (SIRET) inspected. |
| `food_last_inspection_date` | Date of the latest food-safety inspection (ISO). |
| `food_last_inspection_outcome` | Outcome of the latest inspection as published (e.g. Tres satisfaisant, Satisfaisant, A ameliorer, A corriger de maniere urgente). |
| `food_last_inspection_activity` | Activity type of the latest inspected establishment. |
| `rge_qualifications_count` | RGE (ADEME) qualification lines held by the company's establishments. |
| `rge_establishments_count` | Distinct establishments (SIRET) with an RGE qualification. |
| `rge_domains` | RGE domains, separated by semicolons. |
| `rge_latest_start_date` | Latest RGE qualification start date (ISO). |
| `rge_latest_end_date` | Latest RGE qualification end date; null when the source gives none (2099 placeholder removed). |
| `epv_label` | True if the company holds the Entreprise du Patrimoine Vivant label, else null. |
| `epv_universe` | EPV craft universe. |
| `epv_label_start_date` | EPV label start date (ISO). |
| `epv_label_end_date` | EPV label end date (ISO). |
| `supplier_label_category` | Label Relations Fournisseurs et Achats Responsables: size category (PME, ETI, GE). |
| `supplier_label_scope` | Perimeter covered by the supplier-relations label. |
| `last_activity_date` | Most recent dated event among DECP notification, BODACC notice and food inspection (ISO, never in the future). Used by watch mode. |

## Data, licence and attribution
Cross-source company card, one row per SIREN, built by joining on SIREN (first 9 digits of a SIRET). Spine: INSEE SIRENE legal-unit extract (Licence Ouverte 2.0), legal entities only. Sources joined: (1) DECP public-contract winners aggregated by supplier SIREN (data.gouv.fr, Licence Ouverte 2.0; notification dates up to 2026-07); (2) BODACC business-sale notices (buyer is a legal entity, 2023-07 to 2026-07) and BODACC creation/strike-off notices (2025-08 to 2026-07) published by DILA (Licence Ouverte 2.0); (3) Alim'confiance official food-safety inspections, DGAL, Licence Ouverte (Etalab), export of 2026-11-09, an inspection window of about 12 months, aggregated from SIRET to SIREN; (4) RGE qualified companies, ADEME data-fair 'liste-des-entreprises-rge-2', Licence Ouverte / Open Licence, snapshot 2026-11-10 (157,398 qualification lines); (5) Entreprises du Patrimoine Vivant (EPV) label, Ministere de l'Economie, Licence Ouverte v2.0 (Etalab), 1,266 sites; (6) Label Relations Fournisseurs et Achats Responsables, Ministere de l'Economie, Licence Ouverte v2.0 (Etalab), 152 labelled organisations. A SIREN is kept only if it appears in at least two of the six source families (bodacc, decp, alimconfiance, rge, epv, rfar) and in the SIRENE legal-entity extract; a source that does not know the company leaves its columns null (null is never 0). Counts describe what each published dataset contains for the window stated, not the whole life of the company: the absence of an inspection, notice or contract is not proof that none exists. Contract amounts are declared amounts, not revenue. Official public data aggregated by SIREN; this is not a credit report or a legal opinion. Persons: individual entrepreneurs are excluded (SIRENE legal-form codes 1xxx; SIRENs absent from the SIRENE legal-entity extract are dropped); rows whose legal name starts with a civility title, is a short purely alphabetic name containing a common first name or surname (INSEE first-name and family-name files, used only to exclude), or is a short alphabetic name of a single-member company (EURL/SASU) are dropped. No e-mail, telephone, director or inspector name is kept. Business aid schemes (DGE) were not joined because they have no SIREN key.

**Licence:** Licence Ouverte 2.0 (Etalab) / Licence Ouverte (Etalab) / Open Licence, as stated by each source

**Attribution:** Sources: INSEE Base Sirene; DILA BODACC; data.gouv.fr DECP; DGAL Alim'confiance; ADEME liste des entreprises RGE; Ministere de l'Economie EPV and Label Relations Fournisseurs et Achats Responsables. All under Licence Ouverte (Etalab) / Open Licence. Aggregated by SIREN.

Snapshot date: 2026-11-11. Empty values are `null`, never 0.

## Cite this dataset
GitHub's *Cite this repository* button reads [`CITATION.cff`](CITATION.cff).

## Feedback
Found an error or want a field added? Open an issue in this repository.

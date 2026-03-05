# Strategic Patient Risk Stratification & Readmission Predictive Modeling

**Client:** Vitality Health Network (VHN)  
**Team:** The Bug Hunters (Health Informatics Consultants)  
**Context:** Final Coursework (Python) - ITS 2122: Python for Data Science & AI  

---

## 📌 Executive Summary

Vitality Health Network (VHN) faces a critical operational and financial challenge: the 30-day readmission rate for diabetic patients has reached 18%, exceeding acceptable benchmarks and triggering financial penalties under the CMS Hospital Readmissions Reduction Program (HRRP). 

To tackle this, a data science approach was applied to over 100,000 patient encounters from the "Diabetes 130-US Hospitals" dataset. The analysis highlights key clinical and operational indicators that drive readmissions, such as changes in diabetes medications, insulin therapy, and discharge destination (e.g., transfers to Skilled Nursing Facilities). 

As a primary deliverable, we developed the **Vitality Complexity Index (VCI)**, a predictive scoring framework that accurately stratifies patients into low, medium, and high-risk categories to enable targeted transitional care interventions.

---

## 🛠 Methodology

Our end-to-end analytical pipeline involves two core phases to transition raw clinical logs into an executive-grade analytical asset.

1. **Data Ingestion & Clinical Sanitation:**
   - Standardized missing or null values (e.g., "?").
   - Filtered out encounters resulting in patient death to ensure statistical validity (removing Discharge IDs 11, 19, 20).
   - Deduplicated records to prevent skewed representation and statistical noise.

2. **Data Enrichment via Web Scraping:**
   - Transformed abstract ICD-9 diagnostic codes into human-readable conditions by programmatically scraping an external medical coding repository (`icd9data.com`).
   - Focused on the Top 20 most frequent diagnoses for high-impact analysis (e.g., mapping "428" to "Heart Failure").

---

## 📊 Key Clinical & Operational Insights

- **The Investigation-Stay Paradox:** Longer hospitalizations and a high volume of diagnostic lab tests correlate with higher readmissions, indicating patient instability rather than a protective "thoroughness."
- **Medication Adjustments:** Patients experiencing changes to their medication type or dosage during their stay faced notably higher readmission rates (11.97%) compared to those with stable regimens (10.74%).
- **Insulin Dependency:** Patients requiring insulin therapy exhibit greater disease progression and are at a significantly higher risk for readmission.
- **Discharge Disposition:** Discharge destination is a critical vulnerability point. Patients sent to Skilled Nursing Facilities (SNFs) returned at a 14.66% rate, compared to 9.3% for standard home discharges.
- **Demographic Vulnerabilities:** Readmission risk increases significantly for patients aged 60 and above, peaking in the 70–80 age bracket.

---

## 📈 The Vitality Complexity Index (VCI)

To operationalize our findings, we engineered the **VCI**, an adapted scoring framework inspired by the clinically validated LACE Index. It calculates patient complexity and the probability of early readmission via four components:

- **L - Length of Stay Score:** Longer durations inherently map to increased acuity (e.g., `>= 14 days` = 7 points).
- **A - Acuity of Admission Score:** Emergency and trauma admissions are assigned higher risk weights.
- **C - Comorbidity Burden Score:** Evaluated through the volume of recorded diagnoses (e.g., `>= 8 diagnoses` = 5 points).
- **E - Emergency Visit Intensity Score:** Accounts for frequent ED utilization in the year prior to admission.

### Risk Stratification Results
Applying the VCI score isolates the most vulnerable populations clearly:
- **Low Risk (VCI < 7):** 8.81% Readmission Rate
- **Medium Risk (VCI 7–10):** 11.41% Readmission Rate
- **High Risk (VCI > 10):** 14.73% Readmission Rate

---

## 💡 Strategic Recommendations

1. **EHR Integration for Early Risk Alerts:** Embed the VCI directly into the Electronic Health Record system to visually flag High-Risk (VCI > 10) patients within 24 hours of admission.
2. **Enhanced Discharge Protocols:** Mandate thorough discharge instructions, strict medication reconciliation, and 48–72 hour follow-up appointments for patients scoring Medium- or High-Risk, or those admitted through emergency pathways.
3. **Targeted Medication Monitoring:** Deploy pharmacist-led follow-up calls or telehealth check-ins within the first week of discharge for patients prescribed insulin or those whose medications were recently changed.

---

## 🗄️ Dataset Attributes (Data Dictionary Overview)
- `encounter_id`: Unique admission identifier.
- `patient_nbr`: Unique patient identifier.
- `race` & `gender`: Demographics mapping.
- `admission_type_id`: Identifier for admission source (Emergency, Elective).
- `time_in_hospital`: Count of days between admission and discharge.
- `num_lab_procedures` & `num_medications`: Clinical intensity tracking.
- `number_emergency`: Number of ED visits in the prior year.
- `readmitted`: Target variable (`NO`, `>30`, `<30` days).

---
*Prepared for the Vitality Health Network Executive Board.*
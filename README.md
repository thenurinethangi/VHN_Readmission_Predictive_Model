# Strategic Patient Risk Stratification & Readmission Predictive Modeling

Vitality Health Network (VHN) faces a critical operational and financial challenge: the 30-day readmission rate for diabetic patients has reached 18%, exceeding acceptable benchmarks and triggering financial penalties under the CMS Hospital Readmissions Reduction Program (HRRP). 

This project is a predictive analysis model designed specifically for VHN. The analysis goes beyond simple reporting by comprehensively identifying the various clinical and operational factors that contribute to an increase in readmissions. To make these insights actionable, we developed the Vitality Complexity Index (VCI)—a single, easy-to-track score that predicts readmission risk for individual patients. Grounded in these findings, we provide targeted, strategic recommendations to actively reduce readmission rates.

---

## Methodology

Our end-to-end analytical pipeline involves two core phases to transition raw clinical logs into an executive-grade predictive model.

1. **Data Ingestion & Clinical Sanitation:**
   - Standardized missing or null values.
   - Filtered out encounters resulting in patient death to ensure statistical validity.
   - Deduplicated records to prevent skewed representation and statistical noise.

2. **Data Enrichment via Web Scraping:**
   - Transformed abstract ICD-9 diagnostic codes into human-readable conditions by programmatically scraping external medical coding repositories.
   - Focused on the most frequent diagnoses for high-impact analysis (e.g., mapping "428" to "Heart Failure").

---

## Key Clinical & Operational Insights

Our analysis investigated numerous variables to uncover the root causes of readmissions.

### Overall Readmission Landscape
The initial analysis identified the overall distribution of readmission outcomes across the patient population.

![Readmission Landscape](charts/readmission_landscape_chart.png)

### The Investigation-Stay Paradox
Longer hospitalizations and a high volume of diagnostic lab tests correlate with higher readmissions, indicating patient instability rather than a protective "thoroughness."

![Time in Hospital by Readmission Status](charts/time_in_hospital_by_readmission_status_chart.png)

![Average Lab Procedures vs Time in Hospital](charts/average_lab_procedures_vs_time_in_hospital_chart.png)

### Medication Adjustments and Insulin Dependency
Patients experiencing changes to their medication type or dosage during their stay faced notably higher readmission rates compared to those with stable regimens. Additionally, patients requiring insulin therapy exhibit greater disease progression and are at a significantly higher risk for readmission.

![Readmission Rate by Medication Type](charts/readmission_rate_by_medication_type_chart.png)

![Readmission Rate of Medication Change](charts/readmission_rate_of_medication_change_chart.png)

### Discharge Disposition
Discharge destination is a critical vulnerability point. Patients sent to Skilled Nursing Facilities (SNFs) returned at a significantly higher rate compared to standard home discharges.

![Readmission Rate by Discharge Disposition](charts/readmission_rate_by_discharge_disposition%20chart.png)

### Demographic Vulnerabilities
Readmission risk increases significantly for patients aged 60 and above, peaking in the 70–80 age bracket. Variations were also observed across different race and gender intersections.

![Age Distribution](charts/age_distribution_chart.png)

![Readmission Rate by Race and Gender](charts/readmisson_rate_by_race_%26_gender_chart.png)

### Feature Correlation
A correlation heatmap of numerical features was utilized to model the compounding interactions between clinical and operational metrics.

![Correlation Heatmap](charts/correlation_heatmap_of_numerical_features_chart.png)

---

## The Vitality Complexity Index (VCI)

To easily track high-risk patients, we engineered the VCI, a predictive scoring framework that calculates patient complexity and the probability of early readmission. Rather than continuously monitoring dozens of individual factors, clinicians can use this single score derived from four variables:

- **Length of Stay Score:** Longer durations inherently map to increased acuity.
- **Acuity of Admission Score:** Emergency and trauma admissions are assigned higher risk weights.
- **Comorbidity Burden Score:** Evaluated through the volume of recorded diagnoses.
- **Emergency Visit Intensity Score:** Accounts for frequent ED utilization in the year prior to admission.

### Risk Stratification Results
Applying the VCI score isolates the most vulnerable populations clearly. The analysis of readmission risk by category confirms the index's predictive validity.

![Readmission Rate of Risk Category](charts/readmission_rate_of_risk_category_chart.png)

- **Low Risk (VCI < 7):** Represents the lowest readmission probability.
- **Medium Risk (VCI 7–10):** Represents a moderate increase in structural risk.
- **High Risk (VCI > 10):** Represents the most vulnerable patients with the highest likelihood of early returning to the hospital.

---

## Strategic Recommendations

Rather than simply reporting the descriptive statistics, we formulated actionable interventions derived directly from the model's insights to actively reduce readmissions.

1. **EHR Integration for Early Risk Alerts:** Embed the VCI directly into the Electronic Health Record system to visually flag High-Risk patients within 24 hours of admission, enabling proactive care.
2. **Enhanced Discharge Protocols:** Mandate thorough discharge instructions, strict medication reconciliation, and 48–72 hour follow-up appointments for patients scoring Medium- or High-Risk, or those admitted through emergency pathways.
3. **Targeted Medication Monitoring:** Deploy pharmacist-led follow-up calls or telehealth check-ins within the first week of discharge for patients prescribed insulin or those whose medications were recently changed.

---

## Dataset Attributes (Key Variables)
- `encounter_id`: Unique admission identifier.
- `patient_nbr`: Unique patient identifier.
- `race` & `gender`: Demographics mapping.
- `admission_type_id`: Identifier for admission source (Emergency, Elective).
- `time_in_hospital`: Count of days between admission and discharge.
- `num_lab_procedures` & `num_medications`: Clinical intensity tracking.
- `number_emergency`: Number of ED visits in the prior year.
- `readmitted`: Target variable (`NO`, `>30`, `<30` days).
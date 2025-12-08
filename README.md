# Propofol Dosage Analysis - IU Health Ball Memorial Hospital

# Project Background
IU Health Ball Memorial Hospital (BMH), a leading regional teaching hospital within the Indiana University Health system, serves as a major referral center for East Central Indiana. Collaborating with the Director of Pharmacy Services and the Critical Care Research Team, I conduct an analysis to evaluate sedation and pain management patterns in the ICU, specifically examining why propofol infusion rates at BMH are consistently higher than those observed across other IU Health hospitals. Using data extracted from the electronic medical record (EMR) system, this project explores patient characteristics, co-administered sedatives, and propofol outcome measures to uncover patterns driving high-dose propofol use. The resulting insights will inform recommendations to align clinical practice with evidence-based guidelines and support safer, more consistent sedation strategies across the health system.

Insights and recommendations are provided on the following key areas:

•	**Propofol Outcome Measures:** An analysis of the duration and frequency of high dose propofol use (>50 mcg/kg/min), including total infusion hours/days on infusion.

• **IU Health Hospitals Comparisons:** An evaluation of propofol and co-administered sedatives across IU Health hospitals to identify system-wide variation.

•	**Patient Characteristics Analysis:** Evaluation of demographic factors: sex, age, height, and weight.


The R code used to create the cleaned analysis dataset, perform the exploratory data analysis, perform variable selection, fit the GLM, and evaluate evidence of interaction can be found here: [Propofol Dosage Analysis R Code](https://github.com/jasminsc16/propofol-dosage-analysis-IU-Health-Ball-Memorial-Hospital/blob/main/Propofol%20Dosage%20Analysis%20IU%20Health%20Ball%20Memorial%20Hospital.Rmd)



# Data Structure & Initial Checks

The hospital's extracted data from the electronic medical record (EMR) system as seen below consists of one table with a total row count of 72 and a total column count of 44. IBW, COVID, IVDA_Hx, Triglyc_Max, Creatinine, CrCl, Map_Range, and HR_Range_low will all be dropped from the final analysis dataset since they are not relevant to the analysis being completed. So, the official total column count is 36. A description of the table can be found here: [Propofol Dosage Analysis Data Structure Table](https://github.com/jasminsc16/propofol-dosage-analysis-IU-Health-Ball-Memorial-Hospital/blob/main/Propofol%20Project_Data%20Structure%20Table.jpeg)

To prepare the dataset for analysis, an initial data cleaning process was conducted. Variables not relevant to the study’s objectives — including IBW, COVID, IVDA_Hx, Triglyc_Max, Creatinine, CrCl, Map_Range, and HR_Range_low — were removed to streamline the analysis dataset. Placeholder entries “x” and “X” were standardized to NA values to ensure consistency in identifying missing data.

Patterns of missingness were then visualized using vis_miss(), and appropriate replacements were applied based on variable context (e.g., “Missing” for Height and “NotApp” for non-applicable clinical measures such as Min_Rass, Dex_ord, and Prop_dose_when_Dex).

Prior to this stage, the dataset was recovered from a corrupted RDS file and automatically transferred to Excel using an AI-assisted data extraction workflow (Claude), ensuring data integrity and preserving all relevant records for subsequent analysis.



# Executive Summary

This analysis reveals significant variation in sedation practices across IU Health hospitals, with IU HBMH consistently managing a more complex patient population requiring higher opiate doses, advanced sedation strategies (ketamine, NMBAs), and longer propofol durations. The data suggests that scheduled pain medications reduce high dose propofol duration by approximately 60%. Yet, this proactive analgesia approach is underutilized at IU HBMH despite their high opiate-tolerant patient population, suggesting an opportunity for protocol optimization. System-wide, the low adoption of propofol-sparing agents like ketamine (15.2% utilization) and variability in multimodal analgesia practices indicate the need for standardized sedation protocols, pharmacy consultation integration, and staff education to improve patient outcomes while potentially reducing propofol-related complications and costs associated with prolonged sedation.

# Insights Deep Dive

## **Propofol Measure Outcomes:**

•	**IU HBMH had the highest number of fentanyl orders before propofol rate was >50 mcg/kg/min.** IU HBMH had 24 counts of fentanyl orders for patients before reaching high-dose propofol, compared to 11 at IU HMH and 4 at IU HAH. This suggests a more aggressive opioid-first approach to sedation/analgesia at IU HBMH, which aligns with analgesia-first sedation protocols that prioritize pain control before escalating sedative doses. This pattern may indicate better adherence to multimodal analgesia strategies or differences in patient acuity requiring more aggressive pain management.


•	**Only patients at IU HMH and IU HUH received scheduled pain medications when propofol >50 mcg/kg/min. Rec_pain_med (0) (no scheduled pain medications received) has a higher percent of time on propofol.** Patients receiving scheduled pain medications spent roughly 60% less time at high propofol doses compared to those without scheduled analgesia. (median 9 minutes vs. 22 minutes). Scheduled pain medications provide consistent baseline analgesia, potentially reducing propofol requirements and preventing the dose escalation often seen when treating both agitation and unaddressed pain with sedatives alone. This represents a proactive approach versus a reactive approach to patient comfort. The absence of scheduled pain medications at IU HBMH despite higher fentanyl use suggest a PRN (as-needed) approach, which may lead to periods of inadequate analgesia and subsequent propofol escalation.


•	**Most patients were on propofol for less than 10,000 minutes (which is roughly less than a week) with a few higher outliers.** The median duration was 977 minutes (approximately a little over half of a day), with 40 patients receiving propofol for <7 days. However, 2 patients were close to or exceeded 14 days (20,160 minutes), with the longest duration being 29,125 minutes (approximately 20 days). Shorter propofol durations align with ICU best practices for early sedation cessation and liberation from mechanical ventilation. Extended propofol use beyond 7 days raises concerns about propofol infusion syndrome risk, delayed awakening, and potential ICU-acquired weakness. The outliers warrant investigation. Were these patients with complex medical conditions, difficult-to-wean ventilatory requirements, or cases where alternative sedation strategies should have been considered? Understanding these prolonged cases could inform protocol development for sedation rotation or alternative agent selection.


***Duration of time (in minutes) patients are on propofol. Most patients received propofol for <7 days. However, 2 patients were close to or exceeded 14 days (20,160 minutes), with the longest duration being 29,125 minutes.***
<img width="1344" height="960" alt="Propofol Outcome Measures Image" src="https://github.com/user-attachments/assets/a2998930-6fe9-422f-bf17-18f7afd4b289" />


## **IU Health Hospitals Comparisons:**

•	**IU HBMH has the most patients with high tolerance of opiates.** 12.5% of patients at IU HBMH demonstrated high opiate tolerance (defined as “3” in the dataset). High opiate tolerance may reflect differences in patient populations, such as higher rates of chronic pain conditions, substance use disorders, or oncology patients requiring ICU care. This tolerance necessitates higher analgesic doses to achieve adequate pain control and can complicate sedation management, potentially contributing to the higher propofol doses observed at this hospital. The concertation of opiate-tolerant patients at IU HBMH suggests either referral patterns directing complex pain management cases to this facility, or community-level factors affecting the patient demographic. This finding has implications for pharmacy stocking, pain service consultation availability, and staff education on managing tolerance and preventing inadequate analgesia.


•	**Concurrent propofol & ketamine infusion was not a popular choice at the hospitals. Propofol was only decreased to <50 mcg/kg/min after ketamine was added or dose increased at IU HBMH.** Overall, only 15.2% of patients across all facilities received concurrent propofol-ketamine infusions, with IU HBMH and IU HMH accounting for 36.4% of these cases respectively (both had 4 patients receive them concurrently). Ketamine’s use as a propofol-sparing agent leverages its unique NMDA receptor antagonism, providing analgesia and dissociative sedation through a different mechanism. This multimodal approach can reduce propofol requirements, potentially lowering risks of propofol infusion syndrome and hemodynamic instability. The low utilization rates suggest either lack of familiarity with ketamine infusions in ICU settings, concerns about side effects (emergence reactions, increased secretions), or absence of standardized protocols for propofol-ketamine co-administration. IU HBMH’s willingness to use ketamine as a rescue agent for high propofol doses suggests more progressive sedation management or greater institutional experience with ketamine infusions. The successful propofol dose reduction following ketamine addition provides evidence for developing system-wide protocols incorporating ketamine earlier in the sedation escalation pathway, rather than as a last resort.


•	**IU HBMH had the most patients that received concurrent propofol and NMBA infusion.** 13 patients at IU HBMH received concurrent propofol-NMBA infusions, followed by 8 patients at IU HMH and 8 patients at IU HBloomH. Neuromuscular blocking agents (NMBAs) are reserved for the most critically ill patients, typically those with severe ARDS requiring ventilator synchrony, refractory hypoxemia, or elevated intracranial pressure. Adequate deep sedation with propofol is essential during paralysis since patients cannot communicate discomfort or awareness. The higher NMBA use at IU HBMH may indicate either higher patient acuity, more aggressive ARDS management following PROSEVA trial evidence, or differences in mechanical ventilation strategies across facilities. The concentration of NMBA use at IU HBMH raises questions about case mix and patient acuity. Are sicker patients preferentially transferred here, or does the facility have specialized capabilities (ECMO, advanced ventilation modes) attracting complex respiratory failure cases? Additionally, NMBA use requires careful sedation depth monitoring (ideally with processed EEG or BIS monitoring) to prevent awareness during paralysis. Standardization of sedation protocols during NMBA infusions and daily sedation/paralysis interruption trials should be evaluated across all facilities to ensure best practices.


***Concurrent propofol and ketamine infusion across IU Health Hospitals. This was not a popular choice across the hospitals.***
<img width="1344" height="960" alt="IU Health Hospitals Comparisons Image" src="https://github.com/user-attachments/assets/be7d7d4a-17cc-45e6-9b9b-bcaa1d8c6c09" />


## **Patient Characteristics Analysis:**

•	**Most patients’ weights are evenly distributed between 50 kg (roughly 110 lbs) and 150 kg (roughly 330 lbs). There is an outlier of 48 kg (roughly 105 lbs) and six outliers being higher than 150 kg (330 lbs), two falling in the 200 kg (440 lbs) range (one being treated at IU HBMH).** Weight-based dosing is standard practice for propofol and other sedatives, making accurate weight documentation critical. The wide weight distribution (48-200+ kg) presents dosing challenges—lower weight patients may be at higher risk for oversedation at standard doses, while higher-weight patients often require dose adjustments that account for both total body weight and ideal body weight depending on the medication’s pharmacokinetics (how the body interacts with an administered drug over time). Propofol, being lipophilic (easily absorbed in lipids (fats)), has complex distribution in obesity that may necessitate dosing calculations based on adjusted body weight rather than total body weight. The presence of patients in extreme weight categories (both low and high) highlights the need for individualized dosing protocols rather than one-size-fits-all approaches. The higher-weight patients, particularly those exceeding 200 kg, may require specialized equipment, careful hemodynamic (blood flow) monitoring during propofol administration, and potential consultation with pharmacy for optimal dosing strategies. The underweight outlier (48 kg) warrants investigation into whether this represents a malnourished patient or data entry error, as malnutrition can affect drug metabolism and clearance.


•	**Most patients are around 180 cm in height (roughly 5 ft 11 in).** This is notably taller than the general U.S. population average of approximately 170 cm (5 ft 7 inches) overall, though closer to the male average of 175 cm (5 ft 9 in). Height is essential for calculating ideal body weight (IBW), which informs ventilator settings, medication dosing, and assessing nutritional status via BMI. The height distribution supports that the sample is predominantly male. Accurate height measurement in ICU patients can be challenging when patients cannot stand, requiring estimation methods or arm span measurements that may introduce variability. The relatively homogeneous height distribution simplifies some aspects of standardized care protocols (such as ventilator settings). However, any patients at height extremes would require particular attention to ensure IBW-based calculations are used appropriately rather than defaulting to actual body weight, especially for mechanical ventilation parameters where inappropriate tidal volumes can cause ventilator-induced lung injury.


•	**Most patients are between 50 and 70 years old.** The youngest patient was 18 years old, and the oldest patient was 84 years old. This age distribution is consistent with typical ICU demographics, as critical illness requiring mechanical ventilation and deep sedation is more common in older adults with accumulated comorbidities (presence of two or more diseases). Age-related physiological changes affect propofol pharmacokinetics (how the body interacts with an administered drug over time) and pharmacodynamics (what an administered drug does to the body)—older patients typically have decreased hepatic (liver) blood flow, reduced clearance, and increased sensitivity to sedative effects, potentially requiring lower doses to achieve target sedation. Additionally, older adults are at higher risk for delirium, which can be exacerbated by both over- and under-sedation, making careful titration particularly important in this population. The concentration of patients in the 50-70 age range suggests this cohort may benefit from age-specific sedation protocols that account for altered drug metabolism and increased vulnerability to adverse effects. The relatively narrow age distribution also means the sample may not capture the full spectrum of age-related considerations for pediatric or very elderly patients (>80 years old), limiting generalizability to those populations. Exploring whether sedation requirements (propofol doses, duration, etc.) varied by age within this range could inform age-adjusted dosing recommendations.


***Patients' weight distribution at all IU Health Hospitals. Most patients’ weights are evenly distributed between 50 kg (roughly 110 lbs) and 150 kg (roughly 330 lbs).***
<img width="1344" height="960" alt="Patient Characteristics Analysis Image" src="https://github.com/user-attachments/assets/59cedf3e-b428-4b43-9406-749e7246f62e" />



# Recommendations

Based on the insights and findings above, I would recommend the Director of Pharmacy Services and the Critical Care Research team to consider the following:

*Propofol Measure Outcomes:*

•	**Scheduled Analgesia Protocol Implementation.** Implement standardized scheduled analgesia protocols across all IU Health hospitals, with particular emphasis at IU HBMH. The Critical Care Research team should develop guidelines for transitioning from PRN (as-needed administration) to scheduled opioid administration when propofol rates exceed 40 mcg/kg/min, and the Director of Pharmacy Services should ensure adequate stock of long-acting opioid formulations and establish pharmacist-driven pain management consultation triggers for patients requiring high-dose propofol.


•	**Early Sedation Cessation and Duration Monitoring.** Establish facility-wide daily sedation interruption protocols and sedation vacation trials, with mandatory Critical Care Research team review for any patient exceeding 7 days of continuous propofol. The Director of Pharmacy Services should implement automated alerts in the electronic health record when propofol duration reaches 5 days, triggering pharmacist review and recommendations for sedation rotation to alternative agents (dexmedetomidine, ketamine, benzodiazepines) to reduce propofol infusion syndrome risk.


*IU Health Hospitals Comparisons:*

•	**Propofol-Sparing Agent Integration and Ketamine Protocol Development.** The Critical Care Research team should develop and pilot an early multimodal sedation protocol that incorporates ketamine at propofol doses of 40-50 mcg/kg/min rather than waiting for escalation beyond 50 mcg/kg/min. The Director of Pharmacy Services should create ketamine infusion order sets with standardized dosing guidelines, stock adequate ketamine supplies across all hospitals, and provide staff education sessions addressing common concerns about emergence reactions and secretion management. Consider establishing IU HBMH as the pilot site given their existing experience with ketamine as a rescue agent.


•	**Standardization Across High-Acuity Patient Management.** The Critical Care Research team should conduct a comprehensive case-mix analysis to determine whether IU HBMH’s patient population differs significantly in acuity scores (APACHE II, SOFA) or diagnoses from other hospitals. If acuity is comparable, develop system-wide sedation protocols with graduated escalation pathways that can be uniformly applied across all hospitals. The Director of Pharmacy Services should establish a pharmacist-led sedation stewardship program modeled after antimicrobial stewardship, with dedicated ICU clinical pharmacists providing real-time consultation for complex sedation cases and conducting monthly audits of sedation practices at each hospital to identify variation and opportunities for standardization.


*Patient Characteristics Analysis:*

•	**Weight-Based Dosing Protocol Refinement.** The Director of Pharmacy Services should work with the Critical Care Research team to develop and validate weight-stratified propofol dosage guidelines that incorporate adjusted body weight calculations for patients >150 kg and enhanced monitoring parameters for patients <60 kg. Implement mandatory pharmacist consultation for propofol initiation in patients at weight extremes (BMI >40 or <18.5) and establish protocols for processed EEG/BIS monitoring in these populations to objectively assess sedation depth and reduce risks of both undersedation and oversedation.



# Assumptions and Caveats

Throughout the analysis, multiple assumptions were made to manage challenges with the data/dataset. These assumptions and caveats are noted below:
•	**Assumption 1:** Values marked as “x” or “X” in the dataset represent missing or non-applicable data points rather than a specific categorical value and were treated as such throughout the analysis rather than being imputed or excluded.


•	**Assumption 2:** The clinical context and interpretations provided are based on established critical care literature and best practices, but without direct consultation with the IU Health Critical Care Research team, some facility-specific protocols or patient population characteristics may differ from the assumptions made in this analysis.


•	**Assumption 3:** The dataset represents a complete and accurate record of sedation practices during the study period, with the understanding that the original data extraction from the corrupted file maintained data integrity and no transcription errors were introduced during the manual extraction process.

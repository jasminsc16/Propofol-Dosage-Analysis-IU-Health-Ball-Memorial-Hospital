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
# Insights Deep Dive

## **Propofol Measure Outcomes:**

•	**IU HBMH had the highest number of fentanyl orders before propofol rate was >50 mcg/kg/min.** IU HBMH had 24 counts of fentanyl orders for patients before reaching high-dose propofol, compared to 11 at IU HMH and 4 at IU HAH. This suggests a more aggressive opioid-first approach to sedation/analgesia at IU HBMH, which aligns with analgesia-first sedation protocols that prioritize pain control before escalating sedative doses. This pattern may indicate better adherence to multimodal analgesia strategies or differences in patient acuity requiring more aggressive pain management.


•	**Only patients at IU HMH and IU HUH received scheduled pain medications when propofol >50 mcg/kg/min. Rec_pain_med (0) (no scheduled pain medications received) has a higher percent of time on propofol.** Patients receiving scheduled pain medications spent roughly 60% less time at high propofol doses compared to those without scheduled analgesia. (median 9 minutes vs. 22 minutes). Scheduled pain medications provide consistent baseline analgesia, potentially reducing propofol requirements and preventing the dose escalation often seen when treating both agitation and unaddressed pain with sedatives alone. This represents a proactive approach versus a reactive approach to patient comfort. The absence of scheduled pain medications at IU HBMH despite higher fentanyl use suggest a PRN (as-needed) approach, which may lead to periods of inadequate analgesia and subsequent propofol escalation.


•	**Most patients were on propofol for less than 10,000 minutes (which is roughly less than a week) with a few higher outliers.** The median duration was 977 minutes (approximately a little over half of a day), with 40 patients receiving propofol for <7 days. However, 2 patients were close to or exceeded 14 days (20,160 minutes), with the longest duration being 29,125 minutes (approximately 20 days). Shorter propofol durations align with ICU best practices for early sedation cessation and liberation from mechanical ventilation. Extended propofol use beyond 7 days raises concerns about propofol infusion syndrome risk, delayed awakening, and potential ICU-acquired weakness. The outliers warrant investigation. Were these patients with complex medical conditions, difficult-to-wean ventilatory requirements, or cases where alternative sedation strategies should have been considered? Understanding these prolonged cases could inform protocol development for sedation rotation or alternative agent selection.


***Duration of time (in minutes) patients are on propofol. Most patients received propofol for <7 days. However, 2 patients were close to or exceeded 14 days (20,160 minutes), with the longest duration being 29,125 minutes.***
<img width="1344" height="960" alt="Propofol Outcome Measures Image" src="https://github.com/user-attachments/assets/a2998930-6fe9-422f-bf17-18f7afd4b289" />


# **IU Health Hospitals Comparisons:**

•	**IU HBMH has the most patients with high tolerance of opiates.**


•	**Propofol was only decreased to <50 mcg/kg/min after ketamine was added or dose increased at IU HBMH. Concurrent propofol & ketamine infusion was not a popular choice at the hospitals.**


•	**IU HBMH had the most patients that received concurrent propofol and NMBA infusion.**




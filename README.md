# Determinants of Skilled Birth Attendance in Nigeria
### A Population-Based Analysis Using DHS 2018 Data

[![Python](https://img.shields.io/badge/Python-3.9+-blue.svg)](https://www.python.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![DHS](https://img.shields.io/badge/Data-DHS%202018-green.svg)](https://dhsprogram.com/)
[![Status](https://img.shields.io/badge/Status-Complete-success.svg)]()
[![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-orange.svg)](https://jupyter.org/)
DOI: 10.64898/2026.04.23.26350432


---

## 📋 **Table of Contents**
- [Overview](#overview)
- [Key Findings](#key-findings)
- [Background & Motivation](#background--motivation)
- [Research Questions](#research-questions)
- [Data](#data)
- [Methods](#methods)
- [Results](#results)
- [Visualizations](#visualizations)
- [Limitations](#limitations)
- [References](#references)
- [Contact](#contact)
- [Acknowledgments](#acknowledgments)

---

## 🎯 **Overview**

This project analyzes **21,465 births** from the **2018 Nigeria Demographic and Health Survey (NDHS)** to identify independent predictors of skilled birth attendance using **multivariable logistic regression** with proper confounding adjustment.

**Why This Matters:**  
Nigeria accounted for approximately 20% of global maternal deaths in 2017 (WHO, 2019). While recent estimates suggest modest progress (WHO, 2023), the country remains among the world's highest-burden settings for maternal mortality. This 2018 analysis remains highly relevant for understanding structural barriers to skilled delivery care and informing targeted interventions.Understanding barriers to skilled delivery care is critical for reducing preventable maternal mortality. This analysis provides evidence-based insights for policy intervention.

**Key Contribution:**  
Unlike descriptive reports, this analysis **controls for confounding** to isolate independent effects, revealing which factors truly drive access to skilled care versus which associations are explained by other variables.

---

## 🔑 **Key Findings**

| Finding | Statistic | Interpretation |
|---------|-----------|----------------|
| **Overall SBA prevalence** | **44.9%** | Only 4 in 9 Nigerian women deliver with skilled care |
| **Strongest predictor** | Higher education: **aOR = 7.01** (95% CI: 5.68–8.67) | 7× the odds vs no education, after adjustment |
| **Most actionable factor** | ANC ≥4 visits: **aOR = 3.80** (95% CI: 3.51–4.11) | Nearly 4× the odds; modifiable through programs |
| **Wealth gradient** | Richest vs Poorest: **aOR = 6.27** (95% CI: 5.27–7.46) | 6× the odds after controlling for education & residence |
| **Regional inequality** | South West: **85.6%** vs North West: **17.7%** | 68 percentage point gap within one country |
| **Confounding magnitude** | Education crude OR: **64.04** → adjusted: **7.01** | 89% of crude effect was confounding |

### **Policy Implications:**
- **Immediate intervention:** Scale ANC programs (free services, mobile clinics, Community Health Workers (CHWs))
- **Geographic targeting:** Prioritize North West and North East regions
- **Long-term investment:** Girls' education reduces maternal mortality 15-20 years downstream
- **Equity focus:** Address structural wealth barriers to access

---


## 🌍 **Background & Motivation**

### The Problem
**Maternal mortality in Nigeria:**
- **512 deaths per 100,000 live births** (WHO, 2017) — one of the highest globally
- ~67,000 maternal deaths annually — **19% of global total**
- Most deaths are **preventable** with skilled care during delivery

**Skilled Birth Attendance (SBA):**
- Delivery by doctor, nurse, midwife, or auxiliary midwife
- Reduces risk of complications (hemorrhage, infection, obstructed labor)
- Essential for achieving **SDG 3.1** (reduce maternal mortality to <70 per 100,000 by 2030)

### The Gap
While SBA coverage has increased globally, **Nigeria lags behind**. Understanding the **independent determinants** of SBA access is critical for:
- Designing targeted interventions
- Allocating resources efficiently  
- Monitoring health equity

**This analysis goes beyond descriptive statistics** by using multivariable regression to control for confounding, revealing which factors independently predict SBA after accounting for correlated variables.

---

## ❓ **Research Questions**

### Primary Question
**What factors independently predict skilled birth attendance among women of reproductive age in Nigeria?**

### Specific Questions
1. What is the prevalence of skilled birth attendance in Nigeria (2018)?
2. Is maternal education associated with SBA after controlling for wealth and residence?
3. Does household wealth influence SBA independently of education?
4. Is urban residence associated with higher odds of SBA after adjustment?
5. Does attending ≥4 ANC visits increase likelihood of skilled delivery?
6. Which factors remain significant predictors in a fully adjusted model?

### Objectives
1. ✅ Determine the prevalence of SBA in Nigeria
2. ✅ Examine crude (unadjusted) associations between sociodemographic factors and SBA
3. ✅ Assess whether ANC utilization predicts SBA after adjustment
4. ✅ Identify independent predictors after controlling for confounders

---

## 📊 **Data**

### Source
**Nigeria Demographic and Health Survey (NDHS) 2018**  
- Conducted by: National Population Commission (NPC) & ICF
- Sample design: Two-stage stratified cluster sampling
- Nationally representative

### Dataset Details
- **File used:** Individual Recode (IR) — `NGIR7BFL.DTA`
- **Total records:** 41,821 women aged 15–49
- **Analytic sample:** 21,465 women with a birth in the 5 years preceding survey
- **Exclusions:** 
  - 20,029 women with no recent birth (not eligible)
  - 327 women with missing ANC data (1.5% of eligible sample)

### Key Variables

| Variable | DHS Code | Type | Categories |
|----------|----------|------|------------|
| **Outcome** | | | |
| Skilled birth attendance | `m3a_1`, `m3b_1`, `m3c_1` | Binary | 0 = No skilled attendant; 1 = Doctor/nurse/midwife present |
| **Predictors** | | | |
| Education | `v106` | Categorical | No education, Primary, Secondary, Higher |
| Wealth index | `v190` | Categorical | Poorest, Poorer, Middle, Richer, Richest |
| Residence | `v025` | Binary | Urban, Rural |
| Region | `v024` | Categorical | 6 geopolitical zones |
| Age | `v012` | Continuous → Categorical | Grouped: 15–19, 20–24, 25–29, 30–34, 35–39, 40–49 |
| Parity | `v201` | Continuous → Categorical | Grouped: 1, 2–3, 4–5, 6+ children |
| ANC visits | `m14_1` | Continuous → Binary | <4 visits, ≥4 visits (WHO 2016 standard) |
| **Survey Design** | | | |
| Sampling weight | `v005` | Continuous | Applied as `v005/1,000,000` |

### Data Access
The dataset is publicly available upon request from [The DHS Program](https://dhsprogram.com/):

---

## 🔬 **Methods**

### Study Design
**Cross-sectional analysis** of secondary data (2018 NDHS)

### Analytic Approach

#### 1. **Data Preparation**
- Loaded Stata file using `pandas.read_stata()`
- Filtered to women with birth in past 5 years (`m3a_1` non-missing)
- Created binary SBA outcome: `SBA = 1` if doctor OR nurse/midwife OR auxiliary midwife present
- Applied DHS sampling weights: `weight = v005 / 1,000,000`
- Recoded predictors:
  - ANC visits → binary (≥4 vs <4)
  - Age → 6 categories (5-year groups)
  - Parity → 4 categories (1, 2–3, 4–5, 6+)
- Excluded 327 women (1.5%) with missing ANC data

#### 2. **Descriptive Statistics**
- Frequency distributions for all categorical variables
- Mean, median, standard deviation for continuous variables (age, parity)
- Stratified SBA prevalence by each predictor

#### 3. **Bivariate Analysis**
- **Chi-square tests** for association between each predictor and SBA
- **Cramér's V** to measure effect size/strength of association
- Significance level: α = 0.05

#### 4. **Multivariable Analysis**
- **Unadjusted logistic regression:** Each predictor modeled individually
- **Adjusted logistic regression:** All predictors entered simultaneously

**Model specification:**
logit(SBA) = β₀ + β₁(Education) + β₂(Wealth) + β₃(Residence) + β₄(Region) + β₅(Age) + β₆(Parity) + β₇(ANC ≥4)

**Interpretation:**
- Exponentiated coefficients (e^β) = **adjusted odds ratios (aOR)**
- aOR > 1 → increased odds of SBA
- aOR < 1 → decreased odds of SBA
- 95% confidence intervals (CI) calculated using profile likelihood
- p-values from Wald z-tests

**Model fit:** Pseudo R² = 0.40 indicating strong explanatory power for a behavioral health outcome model.

#### 5. **Confounding Assessment**
- Compared crude (unadjusted) vs adjusted odds ratios
- Calculated percent change: `((aOR - crude OR) / crude OR) × 100`
- Magnitude of confounding assessed by reduction in effect size

### Ethical Considerations
- DHS data is de-identified and publicly available
- Analysis conducted for educational/research purposes
- Findings intended to inform public health policy

---

## 📈 **Results**

### 1. Sample Characteristics

**Demographic Profile (n = 21,465):**
| Characteristic | n | % |
|----------------|---|---|
| **Residence** | | |
| Rural | 14,082 | 64.6 |
| Urban | 7,710 | 35.4 |
| **Education** | | |
| No education | 9,527 | 43.7 |
| Primary | 3,410 | 15.6 |
| Secondary | 7,064 | 32.4 |
| Higher | 1,791 | 8.2 |
| **Wealth Index** | | |
| Poorest | 5,025 | 23.1 |
| Poorer | 4,905 | 22.5 |
| Middle | 4,586 | 21.0 |
| Richer | 4,025 | 18.5 |
| Richest | 3,251 | 14.9 |
| **Region** | | |
| North West | 6,309 | 29.0 |
| North East | 4,506 | 20.7 |
| North Central | 3,875 | 17.8 |
| South West | 2,563 | 11.8 |
| South East | 2,365 | 10.9 |
| South South | 2,174 | 10.0 |

**Continuous Variables:**
- Mean age: **29.7 years** (SD = 7.2)
- Mean parity: **4.1 children** (SD = 2.6)

---

### 2. Prevalence of Skilled Birth Attendance

**Overall:** 44.9% (9,795/21,465)

**By Predictor (Crude Rates):**

| Variable | SBA Rate (%) | Chi-square | p-value |
|----------|--------------|------------|---------|
| **Education** | | χ² = 6835.30, df=3 | <0.001 |
| No education | 15.7 | | |
| Primary | 48.1 | | |
| Secondary | 70.8 | | |
| Higher | **92.3** | | |
| **Wealth** | | χ² = 6198.50, df=4 | <0.001 |
| Poorest | **12.2** | | |
| Poorer | 26.2 | | |
| Middle | 49.7 | | |
| Richer | 69.0 | | |
| Richest | 87.2 | | |
| **Residence** | | χ² = 2654.54, df=1 | <0.001 |
| Rural | **32.1** | | |
| Urban | 68.4 | | |
| **ANC Visits** | | χ² = 4746.69, df=1 | <0.001 |
| <4 visits | **17.4** | | |
| ≥4 visits | 64.6 | | |
| **Region** | | χ² = 6129.76, df=5 | <0.001 |
| North West | **17.7** | | |
| North East | 24.8 | | |
| North Central | 54.5 | | |
| South South | 58.0 | | |
| South East | 84.3 | | |
| South West | **85.6** | | |

**Key observations:**
- Perfect dose-response for education and wealth (each step up = higher SBA)
- Massive regional variation (North-South divide)
- Strong ANC gradient (47 percentage point difference)

---

### 3. Multivariable Logistic Regression Results

**Adjusted Odds Ratios (aOR) with 95% Confidence Intervals:**

| Predictor | aOR | 95% CI | p-value |
|-----------|-----|--------|---------|
| **Residence (ref: Rural)** | | | |
| Urban | 1.20 | 1.09–1.31 | <0.001 *** |
| **Education (ref: No education)** | | | |
| Primary | 1.86 | 1.67–2.07 | <0.001 *** |
| Secondary | 2.81 | 2.54–3.12 | <0.001 *** |
| **Higher** | **7.01** | **5.68–8.67** | **<0.001 ***** |
| **Wealth Index (ref: Poorest)** | | | |
| Poorer | 1.63 | 1.44–1.84 | <0.001 *** |
| Middle | 2.63 | 2.33–2.98 | <0.001 *** |
| Richer | 3.71 | 3.24–4.26 | <0.001 *** |
| **Richest** | **6.27** | **5.27–7.46** | **<0.001 ***** |
| **Region (ref: North West)** | | | |
| North Central | 3.54 | 3.17–3.96 | <0.001 *** |
| North East | 1.68 | 1.50–1.87 | <0.001 *** |
| **South East** | **6.55** | **5.62–7.63** | **<0.001 ***** |
| South South | 1.74 | 1.51–2.00 | <0.001 *** |
| **South West** | **6.37** | **5.46–7.43** | **<0.001 ****** |
| **Age Group (ref: 15–19)** | | | |
| 20–24 | 1.12 | 0.93–1.34 | 0.223 NS |
| 25–29 | 1.24 | 1.02–1.50 | 0.031 * |
| 30–34 | 1.53 | 1.24–1.88 | <0.001 *** |
| 35–39 | 1.67 | 1.34–2.08 | <0.001 *** |
| 40–49 | 1.94 | 1.53–2.46 | <0.001 *** |
| **Parity (ref: 1 child)** | | | |
| 2–3 children | 0.63 | 0.56–0.71 | <0.001 *** |
| 4–5 children | 0.57 | 0.49–0.66 | <0.001 *** |
| 6+ children | 0.48 | 0.40–0.57 | <0.001 *** |
| **ANC Visits (ref: <4)** | | | |
| **≥4 visits** | **3.80** | **3.51–4.11** | **<0.001 ****** |

*Significance: *** p<0.001; ** p<0.01; * p<0.05; NS = Not Significant*

**Model Performance:**
- Pseudo R² = 0.40 (McFadden)
- n = 21,465
- All VIF < 3 (no multicollinearity)

---

### 4. Confounding Assessment

**Crude vs Adjusted Odds Ratios:**

| Variable | Crude OR | Adjusted OR | % Change | Interpretation |
|----------|----------|-------------|----------|----------------|
| Higher education | 64.04 | 7.01 | **−89.0%** | Massive confounding by wealth & residence |
| Richest wealth | 48.76 | 6.27 | **−87.1%** | Highly confounded by education & urban residence |
| Secondary education | 13.02 | 2.81 | **−78.4%** | Substantial confounding |
| Urban residence | 4.55 | 1.20 | **−73.7%** | Mostly explained by wealth & education |
| ANC ≥4 visits | 8.69 | 3.80 | **−56.3%** | *Least confounded* — direct mechanism |

**Key Insight:** Education and wealth had enormous crude effects (OR > 60), but 87-89% was due to confounding. After adjustment, effects remain strong but more realistic. ANC showed the least confounding, suggesting a more direct causal pathway.

---

## 📊 **Visualizations**

### 1. Descriptive Analysis

[SBA by Sociodemographic Factors]<img width="2384" height="1535" alt="SBA_descriptive_charts" src="https://github.com/user-attachments/assets/a9b9e1e6-6678-4f30-9924-99e9aed49efb" />


**6-panel chart showing:**
- Wealth gradient (poorest 12% → richest 87%)
- Education staircase (no ed 16% → higher 92%)
- Urban-rural gap (68% vs 32%)
- Regional disparities (North West 18% → South West 86%)
- ANC impact (<4 visits 17% → ≥4 visits 65%)
- Parity decline (1 child 56% → 6+ children 30%)

---

### 2. Inferential Statistics

[Chi-Square Test Results]<img width="1482" height="737" alt="chi_square_results" src="https://github.com/user-attachments/assets/a053b57c-4cd9-46e1-9ec7-c49f00584b43" />


**Bar chart showing:**
- All predictors significantly associated with SBA (p < 0.001)
- Education has largest χ² statistic (6835.3)
- Followed by wealth (6198.5), region (6129.8), ANC (4746.7)
- Age weakest but still significant (χ² = 237.2)

---

### 3. Main Results — Forest Plot

[Forest Plot of Adjusted Odds Ratios]<img width="1484" height="1183" alt="forest_plot_adjusted_ORs" src="https://github.com/user-attachments/assets/2639d7f0-4ac4-4a26-b6bc-ffa50ae0ad04" />


**Publication-quality forest plot showing:**
- Adjusted odds ratios with 95% confidence intervals
- Reference line at OR = 1.0 (no effect)
- Higher education: strongest predictor (aOR = 7.01)
- ANC ≥4 visits: most actionable (aOR = 3.80)
- Urban residence: minimal independent effect (aOR = 1.20)
- All confidence intervals exclude 1.0 (all significant)

---

## ⚠️ **Limitations**

### 1. **Cross-Sectional Design**
- Cannot establish causality (only associations)
- Reverse causation possible for some factors

### 2. **Self-Reported Data**
- Recall bias (women report births up to 5 years ago)
- Social desirability bias (may overreport skilled attendance)

### 3. **Unmeasured Confounding**
Variables not in the model but could influence SBA:
- Facility density/distance to health centers
- Quality of care at facilities
- Cultural/religious norms
- Decision-making autonomy
- Previous birth complications

### 4. **Temporal Limitation**
- Data from 2018; may not reflect current situation (2025)
- COVID-19 pandemic may have disrupted health systems post-2020

### 5. **Missing Data**
- 1.5% missing ANC data (handled via complete case analysis)
- Missing completely at random (MCAR) assumed but not tested

### 6. **Generalizability**
- Findings specific to Nigeria
- May not apply to other sub-Saharan African countries with different health system structures

  ---


📚 References

National Population Commission (NPC) [Nigeria] and ICF. 2019. Nigeria Demographic and Health Survey 2018. Abuja, Nigeria, and Rockville, Maryland, USA: NPC and ICF.


World Health Organization. 2019. Trends in maternal mortality 2000 to 2017: estimates by WHO, UNICEF, UNFPA, World Bank Group and the United Nations Population Division. Geneva: World Health Organization.


World Health Organization. 2016. WHO recommendations on antenatal care for a positive pregnancy experience. Geneva: World Health Organization.


United Nations. 2015. Transforming our world: the 2030 Agenda for Sustainable Development. New York: United Nations.


Say L, Chou D, Gemmill A, et al. 2014. Global causes of maternal death: a WHO systematic analysis. Lancet Global Health 2(6):e323-e333.



📧 Contact
[Ugoeze Lucy Unegbu]
MSc Medical Statistics & Epidemiology
Aspiring Data Engineer in Health
📫 Email: [unegbuugoezelucy@gmail.com]
💼 LinkedIn: https://www.linkedin.com/in/ugoeze-lucy/
📊 GitHub: https://github.com/Ugoeze-Lucy
Open to:
Data Analyst/Scientist roles in healthcare
Collaborations on health data projects
Research partnerships (maternal/child health)
Speaking opportunities (conferences, meetups)



🤝 Acknowledgments
The DHS Program for making high-quality survey data publicly accessible
National Population Commission (Nigeria) for conducting the 2018 NDHS
ICF for technical support and data dissemination
Open-source community for the excellent Python data science ecosystem



📜 License
This project is licensed under the MIT License — see LICENSE file for details.
Data Usage:
DHS data is used in accordance with DHS Program data usage terms. Users must register independently at dhsprogram.com to access the dataset.



⭐ Citation
If you use this analysis in your work, please cite as:
@misc{unegbuugoezelucy2024sba,
  author = {Ugoeze Lucy Unegbu},
  title = {Determinants of Skilled Birth Attendance in Nigeria: 
           A Population-Based Analysis Using DHS 2018},
  year = {2024},
  publisher = {GitHub},
  url = {https://github.com/Ugoeze-Lucy/-Determinants-of-Skilled-Birth-Attendance-in-Nigeria-A-Population-Based-Analysis-Using-DHS-2018-Data/edit/main/README.md}
}



🌟 Support This Work
If you found this analysis useful:
⭐ Star this repository
🐛 Report issues or suggest improvements
🔀 Fork and build upon it (MIT licensed!)
💬 Share with colleagues in public health / data science
📧 Reach out for collaboration

# AI Solution Design Report  
## Smart Patient Triage and Risk Assessment System — Healthcare Sector  

**Part 4 | Module 5 — AI Solution Design for a Business Problem**  
**Prepared By:** AI Business Analyst  
**Version:** 1.0  

---

# Table of Contents
1. Business Domain  
2. Business Problem Definition  
3. AI Task Type  
4. Data Requirement Strategy  
5. Recommended AI Models  
6. Evaluation and Testing Plan  
7. Responsible AI and Ethical Considerations  
8. Executive Summary  

---

# Task 1 — Business Domain

## Domain: Healthcare — Emergency Department Management

### Organisation Background
The proposed solution is designed for a medium-sized city hospital that manages around 350–500 emergency department (ED) visits every day. The hospital faces operational challenges such as limited medical staff, shortage of beds, and increasing patient load during peak hours.

### Why This Domain?
Emergency care requires rapid and accurate decision-making. A small delay or incorrect prioritisation can directly affect patient safety. Triage nurses often work under high pressure and must assess many patients within a short time. An AI-assisted triage system can support healthcare professionals by improving consistency, reducing workload, and helping identify high-risk patients more quickly.

---

# Task 2 — Business Problem Definition

## Problem Statement
When patients arrive at the emergency department, triage nurses assign them a priority level using systems such as the Emergency Severity Index (ESI) or Manchester Triage System (MTS). This process determines how urgently a patient should receive treatment.

The nurse evaluates:
- Vital signs
- Symptoms and chief complaint
- Pain severity
- Existing medical conditions
- Patient history

The complete assessment generally takes between 2–5 minutes per patient.

## Key Stakeholders

| Stakeholder | Main Concern |
|---|---|
| Triage Nurses | Reduce stress and improve assessment accuracy |
| Doctors | Receive properly prioritised patient queues |
| Hospital Management | Improve operational efficiency and patient safety |
| Patients | Faster and fairer treatment |
| Insurance Providers | Avoid unnecessary medical expenses |

---

## Existing Challenges

| Current Issue | Effect |
|---|---|
| Staff fatigue | Accuracy decreases during long shifts |
| Subjective judgment | Different nurses may assign different ESI levels |
| High patient volume | Important symptoms may be missed |
| Limited monitoring after triage | Patient condition may worsen in waiting areas |
| Potential demographic bias | Some groups may be unintentionally under-prioritised |

---

## Business Objectives

| KPI | Current Value | Target Value |
|---|---|---|
| Under-triage rate | 6.2% | ≤ 1.5% |
| Over-triage rate | 11.4% | ≤ 5% |
| Average doctor wait time for urgent patients | 28 min | ≤ 18 min |
| Readmission within 30 days | 8.1% | ≤ 5.5% |
| Triage time per patient | 4.2 min | ≤ 2.5 min |

---

# Task 3 — AI Task Type

## Primary AI Task: Multi-Class Classification

The AI model predicts one of five ESI levels:

| ESI Level | Meaning |
|---|---|
| Level 1 | Immediate life-saving intervention required |
| Level 2 | High risk of deterioration |
| Level 3 | Stable but requires multiple resources |
| Level 4 | Less urgent condition |
| Level 5 | Non-urgent case |

## Secondary AI Task: Anomaly Detection

A second AI component continuously monitors waiting patients and identifies unusual changes in vital signs that may indicate deterioration.

---

## Why Multi-Class Classification?

| Alternative Approach | Reason for Not Choosing |
|---|---|
| Binary Classification | Too simple for medical triage requirements |
| Regression Model | ESI levels are categories, not continuous values |
| Ranking Systems | Hospitals require fixed priority categories |
| Image-Based Detection | Problem is not image-focused |

Multi-class classification aligns directly with existing hospital triage procedures, making the predictions easier for clinical staff to interpret.

---

# Task 4 — Data Requirement Strategy

## Data Sources

| Data Source | Information Type |
|---|---|
| Electronic Health Records (EHR) | Medical history and comorbidities |
| Vital sign records | Blood pressure, heart rate, SpO₂, etc. |
| Chief complaint text | Patient symptoms and concerns |
| Pain score | Severity rating |
| Historical ESI labels | Training targets |
| Lab reports | Blood tests and emergency markers |
| Admission/discharge outcomes | Final patient disposition |

---

## Input Features

### Numerical Features
- Blood pressure
- Heart rate
- Respiratory rate
- Temperature
- Oxygen saturation
- Pain score
- Glasgow Coma Scale (GCS)

### Demographic Features
- Age
- Gender
- BMI

### Clinical Features
- Chief complaint
- Arrival method
- Existing diseases
- Medication count
- Previous ED visits

---

## Target Variable
The prediction target is the historical ESI level assigned during triage.

---

## Minimum Dataset Requirements

| Requirement | Value |
|---|---|
| Total samples | Minimum 50,000 records |
| Historical range | At least 3 years |
| Class balancing | Oversampling for rare emergency cases |
| Missing data handling | Median imputation and uncertainty flags |

---

## Data Quality Challenges

| Risk | Mitigation |
|---|---|
| Missing vital signs | Statistical imputation |
| Inconsistent labels | Senior clinician review |
| Changing disease patterns | Periodic model retraining |
| Sensitive patient information | Data anonymisation |

---

# Task 5 — Recommended AI Models

## Phase 1: Baseline Model

### XGBoost Classifier

| Feature | Details |
|---|---|
| Input | Structured hospital data |
| Training speed | Fast |
| Interpretability | High |
| Hardware need | No GPU required |
| Expected performance | 78–82% accuracy |

This model provides a reliable and interpretable starting point for deployment.

---

## Phase 2: Advanced Production System

### Hybrid NLP + Structured Data Model

The proposed architecture combines:
- **DistilBERT** for analysing patient complaint text
- **Dense neural layers** for numerical and demographic data

### Workflow

```text
Chief Complaint Text → DistilBERT Embedding
                                   ↓
Vital Signs + Patient Data → Feature Combination
                                   ↓
Dense Neural Network
                                   ↓
Softmax Output Layer
                                   ↓
Predicted ESI Level
```

### Advantages
- Handles both text and numerical data effectively
- Captures medical language patterns
- Mimics how clinicians combine symptoms with measured vitals

---

## Phase 3: Longitudinal Monitoring

### LSTM-Based Deterioration Detection

An LSTM model monitors sequential patient vitals every few minutes to detect worsening conditions in waiting patients.

---

## Technology Stack

| Component | Technology |
|---|---|
| Data Processing | Python, Pandas, scikit-learn |
| NLP Pipeline | HuggingFace, spaCy |
| Deep Learning | TensorFlow / PyTorch |
| Deployment | FastAPI, Docker |
| Monitoring | MLflow, Evidently AI |
| Explainability | SHAP, LIME |

---

# Task 6 — Evaluation and Testing Plan

## Technical Evaluation Metrics

| Metric | Target |
|---|---|
| Overall Accuracy | ≥ 82% |
| Macro F1 Score | ≥ 0.80 |
| Recall for Level 1 | ≥ 99% |
| Recall for Level 2 | ≥ 95% |
| Adjacent Accuracy | ≥ 94% |
| Prediction Latency | < 200 ms |

### Important Design Choice
The model intentionally uses a lower decision threshold for Level 1 and Level 2 predictions. This increases sensitivity and reduces the chance of missing critical patients, even if some false alarms occur.

---

## Business Performance Metrics

| Metric | Goal |
|---|---|
| Reduced under-triage | ≤ 1.5% |
| Reduced over-triage | ≤ 5% |
| Faster physician access | ≤ 18 min |
| Lower readmission rate | ≤ 5.5% |
| Faster triage workflow | ≤ 2.5 min |

---

## Deployment Stages

1. **Offline Testing** — Train and validate using historical hospital data  
2. **Shadow Deployment** — AI predictions generated silently alongside nurses  
3. **Decision Support Mode** — AI recommendations shown after nurse assessment  
4. **Full Advisory Mode** — AI suggestions visible during live triage workflow  

---

## Potential Failure Cases

| Failure Scenario | Mitigation |
|---|---|
| Critical patient classified incorrectly | Human review and low decision thresholds |
| Overconfident predictions | Probability calibration |
| Rare symptom patterns | Out-of-distribution detection |
| Sensor errors | Input validation system |

---

# Task 7 — Responsible AI and Ethical Considerations

## Bias and Fairness
Historical healthcare data may contain demographic bias. The model must therefore be tested separately across different age groups, genders, and patient categories.

### Mitigation
- Regular fairness audits
- Performance comparison across demographic groups
- Re-training with weighted loss functions if disparities appear

---

## Incorrect Predictions
An incorrect triage recommendation may seriously affect patient safety.

### Mitigation
- AI remains advisory only
- Nurses retain complete clinical authority
- Confidence scores displayed with every prediction
- Low-confidence outputs require manual assessment

---

## Privacy and Data Security

### Risks
Medical records contain highly sensitive information.

### Protection Measures
- Data anonymisation before training
- Role-based access control
- Secure hospital infrastructure
- No patient identifiers stored in model outputs

---

## Avoiding Over-Reliance on AI

Healthcare staff should not depend entirely on automated recommendations.

### Mitigation
- Regular manual triage practice
- AI presented as “support” rather than “decision-maker”
- Training sessions on identifying incorrect AI outputs

---

## Human Oversight

- Nurses can override any recommendation
- Overrides logged for future improvement
- Weekly review meetings for unusual predictions
- Automatic suspension if repeated harmful errors occur

---

# Task 8 — Executive Summary

## Overview
The proposed AI-based triage support system aims to improve emergency department efficiency and patient safety by assisting nurses in assigning Emergency Severity Index (ESI) levels.

The solution combines:
- NLP analysis of patient complaints
- Structured clinical data analysis
- Real-time monitoring of waiting patients

---

## Expected Outcomes

| KPI | Current | Target |
|---|---|---|
| Under-triage rate | 6.2% | ≤ 1.5% |
| Over-triage rate | 11.4% | ≤ 5% |
| Physician wait time | 28 min | ≤ 18 min |
| Readmission rate | 8.1% | ≤ 5.5% |
| Triage duration | 4.2 min | ≤ 2.5 min |

Estimated benefits include:
- Improved patient safety
- Reduced emergency department congestion
- Better staff workload management
- Lower hospital risk and operational costs

---

## Implementation Timeline

| Phase | Duration |
|---|---|
| Baseline model development | Months 1–2 |
| Hybrid model development | Months 3–4 |
| Pilot deployment | Months 5–6 |
| Full deployment | Months 7–8 |
| Longitudinal monitoring system | Months 9–12 |

---

## Final Note
This system is intended to function as a clinical decision-support assistant rather than an autonomous healthcare tool. Final medical responsibility always remains with trained healthcare professionals.

---

# References

1. Vaswani et al. (2017). *Attention Is All You Need.*  
2. Devlin et al. (2019). *BERT: Pre-training of Deep Bidirectional Transformers.*  
3. Sanh et al. (2020). *DistilBERT.*  
4. Fernandes et al. (2020). Clinical Decision Support Systems for Emergency Triage  
5. DISHA Healthcare Data Protection Draft (India)  
6. Emergency Severity Index (ESI) Implementation Handbook  

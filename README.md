# Part 4 — AI Solution Design for a Business Problem

![Domain](https://img.shields.io/badge/Domain-Healthcare-red)
![AI Task](https://img.shields.io/badge/AI%20Task-Multi--Class%20Classification-purple)
![Project](https://img.shields.io/badge/Project-AI%20Solution%20Design-blue)
![License](https://img.shields.io/badge/License-MIT-brightgreen)

---

# 📌 Project Overview

This project focuses on designing an AI-assisted patient triage system for a hospital Emergency Department (ED). The goal of the solution is to help triage nurses make faster and more consistent decisions while prioritising patients according to their medical condition.

Instead of creating only a simple machine learning model, this project covers the complete AI solution pipeline including:
- Business problem understanding
- Data planning
- Model selection
- Evaluation strategy
- Ethical and responsible AI considerations

The proposed system uses both patient vital signs and written complaints to predict Emergency Severity Index (ESI) levels from 1–5.

| Item | Details |
|---|---|
| **Domain** | Healthcare — Emergency Department |
| **Main Problem** | Delays and inconsistency in manual patient triage |
| **AI Approach** | Hybrid NLP + tabular classification model |
| **Primary Model** | DistilBERT + Dense Neural Network |
| **Goal** | Faster and safer patient prioritisation |

---

# 📂 Repository Structure

```text
part-4-ai-solution-design/
│
├── README.md
├── solution_report.md
├── requirements.txt
├── business_kpi_sample.csv
├── ai_usecase_reference_catalog.csv
│
└── diagrams/
    ├── solution_architecture.png
    ├── kpi_dashboard.png
    ├── evaluation_radar.png
    ├── roadmap_gantt.png
    └── risk_matrix.png
```

---

# 🏥 Task 1 — Business Domain

The healthcare sector is one of the most important areas where AI can create real impact. Emergency departments often handle hundreds of patients every day, and nurses must quickly decide which patient requires immediate attention.

In many hospitals, this process is done manually under stressful conditions. Long working hours, overcrowding, and limited staff can sometimes lead to incorrect patient prioritisation.

This project explores how AI can support healthcare workers by improving triage accuracy and reducing waiting time.

---

# 🩺 Task 2 — Business Problem

Currently, triage nurses assign ESI levels manually based on:
- Vital signs
- Patient symptoms
- Medical history
- Pain level
- Clinical judgement

Although this system works, there are several operational challenges.

| Current Challenge | Impact |
|---|---|
| Under-triage | Critical patients may not receive urgent care |
| Over-triage | Hospital resources may be wasted |
| Long waiting time | Delayed treatment for high-risk patients |
| Staff fatigue | Reduced decision accuracy |
| Subjective judgement | Different nurses may assign different priorities |

### Current Hospital Metrics

| KPI | Current Value |
|---|---|
| Under-triage rate | 6.2% |
| Over-triage rate | 11.4% |
| Level 2 wait time | 28 minutes |
| Readmission rate | 8.1% |

### Stakeholders
- Triage Nurses
- Emergency Doctors
- Hospital Management
- Patients
- Insurance Providers

---

# 🤖 Task 3 — AI Task Type

## Multi-Class Classification

The AI system predicts one of five Emergency Severity Index (ESI) levels.

| ESI Level | Meaning |
|---|---|
| Level 1 | Immediate emergency |
| Level 2 | High-risk condition |
| Level 3 | Urgent but stable |
| Level 4 | Less urgent |
| Level 5 | Non-urgent |

Multi-class classification was selected because hospitals already use this structured framework during triage.

---

# 📋 Task 4 — Data Requirements

The proposed model requires both structured and unstructured healthcare data.

| Data Type | Examples |
|---|---|
| Vital Signs | BP, heart rate, oxygen saturation |
| Patient Details | Age, sex, BMI |
| Chief Complaint | Free-text symptoms |
| Clinical History | Existing diseases, medications |
| Hospital Records | Previous ED visits |

### Target Variable
The prediction target is the historical ESI level assigned by experienced triage nurses.

### Minimum Data Requirement
- At least 50,000 patient records
- Multiple years of hospital data
- Balanced distribution across all ESI levels

---

# 🧠 Task 5 — Proposed AI Model

## Phase 1 — Baseline Model
Initially, an XGBoost classifier can be used on structured hospital data.

### Why XGBoost?
- Fast training
- Easy interpretation
- Works well on tabular healthcare data
- Does not require expensive hardware

---

## Phase 2 — Hybrid Deep Learning Model

The main production system combines:
- DistilBERT for analysing patient complaints
- Dense neural layers for vital signs and demographics

```text
Chief Complaint Text → DistilBERT Embedding
                                   ↓
Vital Signs + Patient Information
                                   ↓
Feature Combination Layer
                                   ↓
Dense Neural Network
                                   ↓
Softmax Output
                                   ↓
Predicted ESI Level
```

### Benefits of This Architecture
- Handles text and numerical data together
- Understands medical language patterns
- Produces more context-aware predictions

---

## Phase 3 — Patient Monitoring System

An LSTM-based model can later be added to continuously monitor waiting patients and detect sudden deterioration in their condition.

---

# 🎯 Task 6 — Evaluation Plan

## Technical Metrics

| Metric | Target |
|---|---|
| Overall Accuracy | ≥ 82% |
| Recall for Level 1 | ≥ 99% |
| Recall for Level 2 | ≥ 95% |
| Adjacent Accuracy | ≥ 94% |
| Inference Time | < 200 ms |

The system prioritises patient safety, so the model is intentionally tuned to minimise under-triage even if it slightly increases false alarms.

---

## Business Metrics

| KPI | Current → Target |
|---|---|
| Under-triage | 6.2% → ≤ 1.5% |
| Over-triage | 11.4% → ≤ 5% |
| Waiting time | 28 min → ≤ 18 min |
| Readmission rate | 8.1% → ≤ 5.5% |

---

# ⚖️ Task 7 — Responsible AI Considerations

AI in healthcare must be carefully monitored because incorrect predictions can directly affect patient safety.

| Risk | Mitigation |
|---|---|
| Bias in training data | Regular fairness audits |
| Wrong predictions | Human review and confidence thresholds |
| Privacy concerns | Data anonymisation and secure storage |
| Over-reliance on AI | Nurses retain final authority |
| Model drift | Continuous monitoring and retraining |

### Human Oversight
The AI system is designed only as a support tool. Final clinical decisions remain with trained healthcare professionals.

---

# 📄 Task 8 — Executive Summary

This project proposes an AI-assisted triage support system for emergency departments. The solution combines NLP and structured clinical data to help hospitals improve patient prioritisation, reduce waiting time, and improve overall emergency care efficiency.

### Expected Improvements

| KPI | Expected Improvement |
|---|---|
| Under-triage reduction | Down to ≤ 1.5% |
| Faster physician access | Reduced to ≤ 18 minutes |
| Lower readmission rates | Down to ≤ 5.5% |
| Improved workflow efficiency | Faster triage process |

### Estimated Benefits
- Better patient safety
- Reduced hospital congestion
- Improved workload management
- Lower operational risk

---

# 🚀 Running the Project

```bash
pip install -r requirements.txt
jupyter notebook notebook.ipynb
```

The notebook generates visualisations and KPI comparison charts using the provided CSV datasets.

---

# 📚 References

1. Vaswani et al. (2017). *Attention Is All You Need.*
2. Devlin et al. (2019). *BERT: Pre-training of Deep Bidirectional Transformers.*
3. Sanh et al. (2020). *DistilBERT.*
4. Fernandes et al. (2020). *Clinical Decision Support Systems for Emergency Triage.*
5. DISHA Healthcare Data Protection Draft (India)
6. Emergency Severity Index (ESI) Handbook

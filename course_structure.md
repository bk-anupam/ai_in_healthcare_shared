# AI in Healthcare — Recommended Course Structure (Revised: Compressed, 16 Weeks)
*B.Tech AI, Third Year — Prepared for [Instructor], Bennett University*

---

## 0. Confirmed parameters

| Field | Value |
|---|---|
| Weekly cadence | 2 × 1-hour lectures + 1 × 2-hour lab, per week |
| Semester length | 16 weeks (today → Dec 31) |
| Lecture hours available | 32 (vs. 45 in the original syllabus — a ~29% cut) |
| Lab hours available | 32 (vs. ~16 assumed earlier — labs are weekly, not alternate) |
| Syllabus flexibility | Confirmed adjustable — new topics can be added, existing ones trimmed |

**New topics added** (your call): Medical Imaging AI for diagnosis, and specialist/generalist medical foundation models (MedGemma, Med-PaLM 2 and similar) for imaging and clinical case support. Both placed in Unit II as applications, with follow-through in Units III–V so they're not just a one-off mention.

This document is the working plan. Existing teaching materials take precedence over historical generation-status statements.

---

## 1. Unit-hour allocation (32 hours / 16 weeks)

| Unit | Original hours | Compressed hours | Weeks |
|---|---|---|---|
| I — Foundations | 9 | 6 | 1–3 |
| II — Applications (+ Imaging AI, + Specialist Medical Models) | 9 | 8 | 4–7 |
| III — Risk & Human-Centered AI | 9 | 6 | 8–10 |
| IV — Development, Validation, Deployment | 9 | 6 | 11–13 |
| V — Regulation, Policy, Future | 9 | 6 | 14–16 |
| **Total** | **45** | **32** | **16** |

Unit II keeps more hours than the rest because it now carries two additional topics — everything else is compressed by merging adjacent original sub-topics into single sessions rather than dropping syllabus content outright.

---

## 2. Session-by-session breakdown

### Unit I — Foundations (6 sessions / 6 hrs, weeks 1–3)

| Session | Topic(s) |
|---|---|
| 1 | Introduction to AI in Healthcare + Historical Evolution of AI |
| 2 | AI/ML/DL Definitions in Healthcare + AI in Non-Healthcare Industries |
| 3 | AI Functions and Capabilities + AI Systems and Data Dependence |
| 4 | Healthcare Data Ecosystem |
| 5 | Key Stakeholders in Healthcare AI |
| 6 | Current State of AI Adoption + Trust, Equity, Inclusion + Opportunities and Challenges *(synthesis)* |

### Unit II — Applications (8 sessions / 8 hrs, weeks 4–7)

| Session | Topic(s) |
|---|---|
| 1 | AI Solutions for Patients and Families + Patient Engagement and Personalized Healthcare |
| 2 | AI Solutions for Clinician Care Teams + Clinical Decision Support Systems |
| 3 | AI for Diagnosis and Treatment Planning *(general — risk scores, EHR-based prediction)* |
| 4 | **[NEW] Medical Imaging AI** — radiology, pathology, dermatology, signal-based diagnosis (e.g. ECG); computer vision in diagnostic workflows |
| 5 | **[NEW] Specialist & Generalist Medical Foundation Models** — MedGemma, Med-PaLM 2, multimodal clinical reasoning; agentic clinician-support (report interpretation, case-diagnosis assistance) |
| 6 | AI Solutions for Population and Public Health + Predictive Healthcare and Disease Surveillance |
| 7 | AI Solutions for Healthcare Business Administrators + Resource Optimization, Workflow Automation, Operations, Quality Improvement |
| 8 | Case Studies of Healthcare AI Applications *(synthesis — include imaging/foundation-model case studies)* |

### Unit III — Risk & Human-Centered AI (6 sessions / 6 hrs, weeks 8–10)

| Session | Topic(s) |
|---|---|
| 1 | Hype versus Hope in Healthcare AI + Benefits and Limitations of AI Systems |
| 2 | Risks of Improper AI Deployment + Patient Safety Concerns |
| 3 | Unintended Consequences of AI in Medicine *(include imaging-specific failure modes: dataset shift, spurious correlations)* |
| 4 | Human–AI Interaction + Patient–Provider–AI Relationships |
| 5 | Trust and Acceptance of Healthcare AI + Liability and Accountability Issues |
| 6 | Impact of AI on Healthcare Workforce + Human-Centered AI Design Principles *(synthesis)* |

### Unit IV — Development, Validation, Deployment (6 sessions / 6 hrs, weeks 11–13)

| Session | Topic(s) |
|---|---|
| 1 | Healthcare AI Development Lifecycle + Data Collection and Data Quality Requirements |
| 2 | Model Development and Training *(tabular vs. imaging model training; fine-tuning/prompting specialist models like MedGemma)* |
| 3 | Model Validation and Performance Assessment + Challenges in Healthcare Data |
| 4 | Clinical Settings for AI Deployment + Applications in Clinical Care Delivery |
| 5 | Frameworks for AI Selection/Implementation + Integration into Clinical Workflows |
| 6 | AI Interpretability and Explainability in Clinical Practice *(incl. saliency maps for imaging models)* + Education/Skill Requirements *(synthesis)* |

### Unit V — Regulation, Policy, Future (6 sessions / 6 hrs, weeks 14–16)

| Session | Topic(s) |
|---|---|
| 1 | Healthcare AI Laws and Regulatory Frameworks *(incl. Software as a Medical Device — relevant to imaging AI)* |
| 2 | Safety and Efficacy of Clinical AI Systems |
| 3 | Privacy, Information and Data Governance *(India: ABDM, DPDP Rules 2025)* |
| 4 | Ethical Considerations + Responsible and Trustworthy AI *(ICMR guidelines)* |
| 5 | Policy Challenges + Future Healthcare AI Ecosystems + Emerging Trends |
| 6 | Course Synthesis: Key Priorities, Hope Not Hype/Promise Not Peril, capstone demonstration & reflection |

---

## 3. Lab plan (16 × 2-hour labs)

| Week | Lab | Topic |
|---|---|---|
| 1 | 1 | Healthcare data exploration: Synthea synthetic records, cohort construction, data dictionary |
| 2 | 2 | Data quality & structured/unstructured handling; intro to FHIR resources |
| 3 | 3 | Reproducible baseline model: cleaning, missingness, leakage-safe train/test split |
| 4 | 4 | Clinical Decision Support prototyping: threshold-based alerting logic |
| 5 | 5 | **[NEW]** Medical imaging classification: CNN/transfer learning on a public de-identified imaging dataset |
| 6 | 6 | **[NEW]** Working with specialist medical models: prompting a MedGemma-class model for image- and text-based case support; compare vs. general-purpose LLM baseline |
| 7 | 7 | Evaluation metrics: sensitivity/specificity/PPV/NPV, ROC/PR curves, threshold trade-offs |
| 8 | 8 | Predictive healthcare / disease surveillance mini-exercise on synthetic population data |
| 9 | 9 | Error analysis and subgroup performance audit |
| 10 | 10 | Calibration and fairness audit across patient subgroups |
| 11 | 11 | Human-centered design exercise: redesigning an alert/interface to reduce alert fatigue |
| 12 | 12 | Explainability: SHAP/permutation importance (tabular) + saliency maps (imaging model from Lab 5) |
| 13 | 13 | Model validation: internal vs. external, patient-level/temporal splitting |
| 14 | 14 | Prototype interface (Streamlit) integrating model + specialist-model output, with stated limitations |
| 15 | 15 | Safety case, threat analysis, privacy/ethics checklist, monitoring plan |
| 16 | 16 | Final capstone demonstration, failure-mode testing, documentation, viva |

Labs 5–6 are the direct build-through of the two new lecture topics — imaging classification and specialist-model prompting — rather than being purely conceptual.

### Available Clinical Decision Support materials

Unit II session 2 uses [the concepts notebook](module2/lectures/session-02-clinical-decision-support-concepts.ipynb) and [Lab 4](module2/labs/lab-04-clinical-decision-support.ipynb). The lecture filename uses the Unit II session number; existing Unit I files are unchanged. Both derive from `module1/labs/medical_predictive_analysis.ipynb`, which remains preserved.

The primary alignment is CO2 (clinical decision support applications), supported by CO3 (errors, human review and limitations) and CO4 (development and validation). Prerequisites are introductory Python, pandas, supervised learning and healthcare data-quality concepts. Use the workflow, score, threshold and review-message examples as the core lecture; detailed EDA, coefficient mathematics, calibration and optional extensions support independent study or later Unit IV reuse. The companion lab is a single 120-minute exercise in the existing Lab 4 slot; it introduces metrics needed for policy selection, with deeper evaluation retained in Lab 7. No lecture-hour allocation changes are implied.

Both notebooks use the original Pima diabetes CSV via a checksum-verified download/cache, with dataset access and licence uncertainty documented in setup. They teach historical label classification in a fictional review workflow, not future diabetes prediction or a validated clinical application. The student lab contains prompts and runnable scaffolding without an answer appendix.

---

## 4. Capstone concept (unchanged in spirit, now with an imaging/specialist-model option)

Default: a risk-screening assistant reading a synthetic patient record, estimating risk, explaining contributing factors, supporting threshold selection, flagging cases where it should abstain, shipped with a monitoring/safety plan.

Given the new content, an equally strong alternative track: an **imaging-based screening assistant** (e.g., a chest X-ray or skin-lesion classifier) paired with a specialist-model component (MedGemma-class model generating a draft finding/explanation) and a clear "not a diagnosis, flag for clinician review" framing — this maps directly onto Labs 5, 6, and 12.

Hard rules unchanged: no real patient-identifiable data; no clinical-readiness claims; sensitivity/specificity/PPV/NPV/calibration required beyond accuracy; patient-level/temporal splitting where applicable; narrow-task + hallucination/grounding evaluation required if any team uses an LLM component.

---

## 5. Assessment scheme (still pending your institution's actual split)

| Component | Suggested weight |
|---|---:|
| Internal Assessment (labs, quizzes, case critiques) | 30% |
| Mid Term Exam | 20% |
| End Exam | 50% |

---

## 6. Data and software stack (updated for imaging + specialist models)

- Python, Jupyter/Colab, pandas, NumPy, scikit-learn, matplotlib/seaborn
- Synthea (synthetic patient records, CSV + FHIR) for data-ecosystem realism
- UCI Breast Cancer Wisconsin dataset for early tabular-modeling mechanics
- A public de-identified medical imaging dataset for Lab 5 (e.g. a chest X-ray or dermatology set — to be finalized based on license/size suitable for classroom compute)
- An open MedGemma-class model (or comparable open multimodal medical model) for Lab 6, run via a hosted API or locally if GPU resources permit
- SHAP / permutation importance + Grad-CAM-style saliency maps for explainability across tabular and imaging models
- Streamlit for prototype interfaces

---

## 7. India-specific governance references (Unit V)

- ICMR Ethical Guidelines for AI in Biomedical Research and Healthcare
- ABDM Health Data Management Policy
- Digital Personal Data Protection Rules, 2025
- CDSCO medical-device/software guidance (verify current version at time of teaching — relevant to imaging AI as a regulated device class)

---

## Continuing material development

Develop remaining materials against the session and lab plan, preserving existing paths and completed materials. The historical confirmation request has been superseded by subsequent material-development requests.

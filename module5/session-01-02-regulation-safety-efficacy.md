# Sessions 1–2 — Regulating Clinical AI: Laws, Frameworks, Safety and Efficacy

**AI in Healthcare · Unit V · B.Tech AI, Third Year**  
**Student lecture notes and primary study material**  
**Previous session:** [Unit IV Session 5 — Explainability and skills](../../module4/lectures/session-05-explainability-skills.md)

## Learning objectives

After studying this chapter, you should be able to:

1. Decide whether a piece of healthcare software is a **medical device**, using its intended purpose.
2. Classify medical device software by risk, using the IMDRF matrix and India's Class A–D system.
3. Compare how India, the United States, and the European Union regulate AI-enabled medical software.
4. Explain how regulators handle models that are updated after approval.
5. Distinguish technical performance, clinical performance, and clinical benefit, and judge which kind of evidence a study provides.
6. Describe how safety is managed before and after a product reaches the market.

You need Unit IV Sessions 3–4 (validation, deployment, inform/drive/diagnose) and Unit III Session 6 (accountability). Medical and legal knowledge is not assumed.

## Contents

1. [The problem: who decides that clinical AI is safe enough?](#1-the-problem-who-decides-that-clinical-ai-is-safe-enough)
2. [Vocabulary: laws, rules, guidance, and standards](#2-vocabulary-laws-rules-guidance-and-standards)
3. [When is software a medical device?](#3-when-is-software-a-medical-device)
4. [Risk-based classification](#4-risk-based-classification)
5. [Three jurisdictions compared](#5-three-jurisdictions-compared)
6. [Regulating a model that changes](#6-regulating-a-model-that-changes)
7. [Efficacy: what evidence shows that it works?](#7-efficacy-what-evidence-shows-that-it-works)
8. [Safety: managing risk before and after the market](#8-safety-managing-risk-before-and-after-the-market)
9. [Case discussion](#9-case-discussion)
10. [Revision summary](#10-revision-summary)
11. [Review questions](#11-review-questions)
12. [Further reading](#12-further-reading)

## 1. The problem: who decides that clinical AI is safe enough?

In Lab 12 you trained a model that labels paediatric chest X-rays as *pneumonia* or *normal*. Suppose (a fictional scenario) a Bengaluru startup builds a similar model on a much larger dataset and wants to sell it to hospitals in India, the United States, and Europe as a tool that **moves suspected pneumonia cases to the top of the radiologist's reading list**.

Before any hospital may buy it, someone has to answer:

- Is this software a **medical device**, or ordinary hospital software?
- If it is a device, **how risky** is it, and therefore how much checking does it need?
- **What evidence** must the company provide — a test-set AUC, a hospital study, a randomised trial?
- What happens when the company **retrains** the model next year?
- Who watches for problems **after** it is sold?

These questions are answered by **regulation**. This chapter covers the first two Unit V topics: the legal frameworks (Sections 2–6) and the evidence of safety and efficacy they require (Sections 7–8). Data protection (DPDP, ABDM) is the subject of Session 3, and ethics guidelines (ICMR) of Session 4.

## 2. Vocabulary: laws, rules, guidance, and standards

Regulatory documents have different legal strength. Mixing them up is a common error.

| Type | What it is | Binding? | Indian example |
|---|---|---|---|
| **Law (Act)** | Passed by Parliament | Yes | Drugs and Cosmetics Act, 1940 |
| **Rules / regulations** | Detailed requirements made by the government under an Act | Yes | Medical Devices Rules, 2017 (MDR-2017) |
| **Guidance** | A regulator's explanation of how it applies the rules | No, but in practice expected | CDSCO *Guidance Document on Medical Device Software* (2026) |
| **Standard** | Technical specification agreed by experts (ISO, IEC, BIS) | Becomes required when rules refer to it | ISO 14971 (risk management), IEC 62304 (software life cycle) |

Three other terms appear throughout:

- **Regulator** — the government body that authorises products. India: **CDSCO** (Central Drugs Standard Control Organisation); US: **FDA** (Food and Drug Administration); EU: independent **notified bodies** check devices against EU law.
- **Intended purpose** (or intended use) — what the manufacturer says the product is for, who uses it, on which patients, and in which setting. Regulation is built around this statement, not around the algorithm.
- **Safety** means the risks are acceptable; **efficacy** (or *effectiveness*) means it achieves its intended purpose. Medical-device regulators usually speak of **safety and performance**.

## 3. When is software a medical device?

### 3.1 The definition

The International Medical Device Regulators Forum (**IMDRF**), whose members include India, the US, and the EU, defines **Software as a Medical Device (SaMD)** as software intended for one or more medical purposes that performs these purposes **without being part of a hardware medical device**.

Medical purposes include diagnosis, prevention, monitoring, prediction, treatment, or alleviation of disease. India's 2026 guidance uses the wider term **medical device software (MDSW)**, covering both:

- **standalone software** (SaMD) — e.g. an app that analyses uploaded X-rays; and
- **software in a medical device (SiMD)** — e.g. the firmware that controls an insulin pump.

### 3.2 The intended purpose decides, not the technology

The same algorithm can be a device or not, depending on what it is claimed to do:

| Software | Medical device? | Why |
|---|---|---|
| AI tool that triages or screens images for cancer lesions | Yes | Detects disease |
| App reading a Bluetooth blood-pressure cuff to track hypertension | Yes | Monitors a condition for a medical purpose |
| Hospital information system for admissions, appointments, billing | No | Administrative data management |
| Software that only stores, transfers, compresses, or displays data | No | No medical purpose of its own |
| Software solely for medical teaching or training | No | Educational purpose |

*Examples are taken from the CDSCO guidance, Sections 5.1–5.2.*

The last row matters for this course: your lab notebooks are **educational prototypes**, not medical devices. That is precisely why they must never be presented as fit for patient care — the moment someone claims a clinical purpose, the regulatory obligations begin.

### 3.3 Course examples

| Course example | Device? |
|---|---|
| Lab 12 pneumonia classifier, *used to flag real patients* | Yes — detects disease from images |
| Heart-disease referral model (Lab 4), *deployed in a clinic* | Very likely — predicts risk and drives referral |
| Operations model forecasting bed occupancy (Unit II) | No — no medical purpose for an individual patient |
| Inbox agent (Unit III) drafting appointment replies | Probably not; but if it judges whether symptoms are urgent, it starts to make a medical decision |

The last row shows why **scope creep** (Unit III) is also a regulatory problem: adding a feature can change the product's legal category.

## 4. Risk-based classification

### 4.1 The IMDRF matrix

Not all medical software needs the same scrutiny. IMDRF classifies SaMD using two questions:

1. **How significant is the information** to the healthcare decision? *Treat or diagnose* > *drive clinical management* > *inform clinical management* (Unit IV Session 4).
2. **How serious is the healthcare situation?** *Critical* (life-threatening or time-critical) > *serious* > *non-serious*.

India's MDSW guidance (Table 2) applies this matrix to its four classes:

| Situation \ Significance | Treat or diagnose | Drive clinical management | Inform clinical management |
|---|---|---|---|
| **Critical** | D | C | B |
| **Serious** | C | B | A |
| **Non-serious** | B | A | A |

Class A is low risk and Class D high risk. (IMDRF's own categories run I–IV in the same pattern, with IV the highest.) The guidance adds that software used by **non-clinical users** in a serious situation, without professional support, may be treated as critical.

### 4.2 Worked example

Classify the startup's chest X-ray tool from Section 1 in two versions:

| Version | Significance | Situation | Class |
|---|---|---|---|
| Reorders the radiologist's list; the radiologist reads every image | Drive (triage) | Serious — pneumonia in children needs timely treatment, but a radiologist still reviews | **B** |
| Reports "no pneumonia" and the image is **not** read by a radiologist | Diagnose | Critical — a missed case in a sick child may be life-threatening | **D** |

The model could be identical in both versions. **Removing the human check moves the product from Class B to Class D.** The final class is confirmed by the regulator from the intended-use statement; CDSCO also publishes a list of already-classified software.

### 4.3 Why the class matters

The class decides who licenses the product and how much evidence is expected. In India, Class A and B manufacturing licences are granted by **State Licensing Authorities**; Class C and D licences, all import licences, and all clinical-investigation permissions are granted by the **Central Licensing Authority** (CDSCO). Higher classes face more detailed review.

## 5. Three jurisdictions compared

The same product must satisfy each market separately. The table summarises the position as checked in October 2026.

| | India | United States | European Union |
|---|---|---|---|
| **Main law** | Drugs and Cosmetics Act 1940; Medical Devices Rules 2017 | Federal Food, Drug, and Cosmetic Act | Medical Device Regulation (EU) 2017/745 **and** AI Act (EU) 2024/1689 |
| **Regulator** | CDSCO | FDA | Notified bodies; national authorities |
| **Risk classes** | A, B, C, D | I, II, III | I, IIa, IIb, III |
| **Software-specific text** | *Guidance Document on Medical Device Software* (21 July 2026) | Guidance on clinical decision support (January 2026), AI-enabled devices, change control plans | MDR classification **Rule 11** for software |
| **AI-specific law?** | No; AI software is covered as medical device software | No; covered as devices | Yes — AI Act adds requirements for high-risk AI |

### 5.1 India

Since a 2020 notification (S.O. 648(E)), **all** medical devices, including software, are regulated under MDR-2017. The July 2026 MDSW guidance does not create a new pathway; it explains how software fits the existing one. Points relevant to AI:

- AI/ML tools, mobile apps, and cloud-hosted software are within scope.
- Manufacturers must state whether training data are real-world or synthetic, and address **bias, generalisability, and robustness across sub-populations relevant to India**.
- Performance evidence from outside India **may be supplemented with validation in representative Indian populations** in the intended clinical environments.
- Post-market surveillance of AI software must include **drift monitoring** (Section 8.3).

### 5.2 United States

The FDA authorises devices through three main routes:

- **510(k)** — show the device is *substantially equivalent* to a legally marketed "predicate" device. Most AI devices use this route.
- **De Novo** — for a novel low-to-moderate-risk device with no predicate. The first autonomous AI diagnostic system, **IDx-DR** for diabetic retinopathy, was authorised this way in April 2018.
- **Premarket approval (PMA)** — for Class III, highest-risk devices; usually requires clinical studies.

Some **clinical decision support (CDS)** software is excluded from the device definition by the 21st Century Cures Act (2016). Broadly, it must (1) not analyse medical images or physiological signals, (2) display or analyse medical information, (3) support a healthcare professional's recommendation, and (4) let that professional **independently review the basis** of the recommendation rather than rely on it. The FDA's January 2026 CDS guidance relaxed its approach to tools giving a single recommendation, but software that analyses medical images remains a regulated device. The chest X-ray tool therefore cannot use this exclusion.

The FDA publishes a [list of AI-enabled devices](https://www.fda.gov/medical-devices/artificial-intelligence-enabled-medical-devices/list-artificial-intelligence-enabled-medical-devices) it has authorised; it runs to well over a thousand entries, the majority in radiology.

### 5.3 European Union

Under the EU MDR, **Rule 11** classifies software that provides information for diagnostic or therapeutic decisions as Class IIa, rising to IIb if a decision could cause serious deterioration of health or surgery, and III if it could cause death or irreversible deterioration.

The **EU AI Act** (in force since 1 August 2024) adds a second layer. An AI medical device that needs a notified-body assessment under the MDR (Class IIa and above) counts as a **high-risk AI system**, with extra duties on data governance, logging, transparency, human oversight, accuracy, and robustness. A 2026 amendment (the "Digital Omnibus on AI", Regulation (EU) 2026/1744) moved the date these duties apply to AI in products such as medical devices from 2 August 2027 to **2 August 2028**. Some AI Act duties already apply, such as the staff **AI literacy** requirement mentioned in Unit IV Session 5.

## 6. Regulating a model that changes

Traditional device regulation assumes a fixed product. Machine-learning models are often **retrained** — on more data, a new hospital's data, or to correct drift. Two kinds of model are distinguished:

- A **locked** model gives the same output for the same input until the manufacturer releases a new version.
- An **adaptive** (continuously learning) model changes its behaviour in use.

Requiring a full new approval for every retraining would discourage improvements; allowing any change without review would make the original approval meaningless. Regulators' answer is a **pre-approved change plan**:

| Jurisdiction | Mechanism |
|---|---|
| United States | **Predetermined Change Control Plan (PCCP)** — final guidance December 2024 |
| India | **Algorithm Change Protocol (ACP)** — in the 2026 MDSW guidance |

India's ACP may contain:

1. a **data management plan** — how new data will be collected and quality-checked;
2. a **performance evaluation and monitoring plan** — metrics, statistical analysis, frequency, targets;
3. an **algorithm retraining plan** — objectives, methods, and impact on the intended purpose;
4. a **software update plan** — version tracking, verification and validation, communication to users;
5. a **rollback plan** — triggers and recovery procedures.

Changes within the approved protocol are notified to or approved by the licensing authority; a change to the **intended use, safety, or effectiveness** is a major change needing prior approval. Notice how closely this mirrors the lifecycle you met in Unit IV Session 1 and the rollback plan in Unit IV Session 4.

### 6.1 Good machine learning practice

The FDA, Health Canada, and the UK's MHRA published ten **Good Machine Learning Practice (GMLP)** guiding principles in 2021; IMDRF adopted almost identical principles in January 2025. In short:

| # | Principle | Where you met it |
|---|---|---|
| 1 | Multidisciplinary expertise across the whole product life cycle | Unit I stakeholders |
| 2 | Good software engineering and security practices | Unit III threats |
| 3 | Study participants and data represent the intended patient population | Unit IV Session 1 |
| 4 | Training data independent of test data | Patient-level splits, Lab 13 |
| 5 | Reference standards based on best available methods | Label construction |
| 6 | Model design suited to the data and intended use | Unit IV Session 2 |
| 7 | Focus on the performance of the **human–AI team** | Unit III Session 5 |
| 8 | Testing in clinically relevant conditions | External validation |
| 9 | Users given clear, essential information | Labelling, Unit IV Session 5 |
| 10 | Deployed models monitored; retraining risks managed | Section 8.3 |

## 7. Efficacy: what evidence shows that it works?

### 7.1 Three kinds of evidence

IMDRF's clinical-evaluation framework, also used in the CDSCO guidance, asks for three things:

| Evidence | Question | Example for the X-ray tool |
|---|---|---|
| **Valid clinical association** (scientific validity) | Is the output linked to the clinical condition? | Chest X-ray findings are an accepted sign of pneumonia |
| **Technical (analytical) performance** | Does the software correctly and reliably produce its output from the input? | Same output for the same image; handles different X-ray machines and image sizes |
| **Clinical performance** (clinical validation) | Does the output give clinically meaningful results in the target population? | Sensitivity and specificity against expert-confirmed diagnoses in Indian paediatric patients |

Beyond these, a buyer may ask about **clinical benefit**: do patients do better, or does the service work better, when the tool is used? That is the hardest question, and usually requires a prospective study.

### 7.2 Study designs, from weakest to strongest

| Design | What it shows | Course link |
|---|---|---|
| Retrospective, single site | Performance on stored data from one place | Lab 12 test set |
| Retrospective, external sites | Whether performance transfers | Unit IV Session 3 (four UCI sites) |
| Silent (shadow) deployment | Performance on live data, without affecting care | Unit IV Session 4 |
| Prospective study with pre-specified endpoints | Performance in real use, with goals set in advance | IDx-DR |
| Randomised controlled trial (RCT) | Effect on outcomes compared with care without AI | MASAI |

**Pre-specified endpoints** mean the success thresholds are fixed before the data are seen, so the study cannot be "tuned" to pass — the same reasoning as not tuning on your test set.

### 7.3 Two examples

**IDx-DR (prospective study, 2018).** At 10 US primary-care sites, 900 people with diabetes were enrolled. The system had to exceed pre-specified goals for detecting more-than-mild diabetic retinopathy (eye damage caused by diabetes): sensitivity above 85% and specificity above 82.5%. It achieved **87.2% sensitivity and 90.7% specificity**, and this study supported FDA authorisation as the first autonomous AI diagnostic system.

What would this mean in a clinic? Suppose (illustrative) 20% of 1,000 patients screened have the condition:

| Quantity | Calculation | Count |
|---|---|---|
| Patients with disease | 1,000 × 0.20 | 200 |
| Correctly detected | 200 × 0.872 | ≈ 174 |
| Missed | 200 − 174 | ≈ 26 |
| Patients without disease | 1,000 × 0.80 | 800 |
| False alarms | 800 × (1 − 0.907) | ≈ 74 |
| Positive predictive value (PPV) | 174 / (174 + 74) | ≈ 70% |

About 3 in 10 referrals would be unnecessary, and 26 patients would be wrongly reassured. Regulators judge whether this balance is acceptable for the intended use — here, referral to an eye specialist rather than final treatment.

**MASAI (randomised trial, Sweden).** More than 100,000 women in the national breast-screening programme were randomised between AI-supported screening and standard double reading by two radiologists. Earlier results (2023) showed the AI arm cut radiologists' screen-reading workload by 44% without reducing cancer detection. Final results in *The Lancet* (2026) reported **about 12% fewer interval cancers** — cancers diagnosed between screening rounds, which suggests they were missed. This is evidence of **clinical benefit**, not just accuracy. It applies to one product used in one way, within a programme that already double-reads.

### 7.4 The evidence gap

Most authorised AI devices have far less evidence than these two. An analysis of 130 FDA-cleared AI devices (Wu et al., 2021) found that **126 had only retrospective evaluations**, **93 had no publicly reported multi-site evaluation**, and none of the 54 high-risk devices had a prospective study. **Regulatory authorisation is not proof of clinical benefit**; ask what evidence it rests on.

Reporting guidelines help readers judge studies: **TRIPOD+AI** for prediction models, **DECIDE-AI** for early clinical evaluations (Unit IV Session 4), and **SPIRIT-AI / CONSORT-AI** for trial protocols and reports.

## 8. Safety: managing risk before and after the market

### 8.1 Risk management

Device makers follow **ISO 14971**, which separates three ideas:

- a **hazard** — a potential source of harm (e.g. the model misses pneumonia);
- a **hazardous situation** — circumstances in which someone is exposed to it (a child's X-ray is placed low in the queue and read hours later);
- **harm** — the injury that results (delayed treatment).

Risk combines the **probability** of harm with its **severity**. The manufacturer lists hazards, estimates risks, adds **controls**, and checks that the remaining risk is acceptable.

| Hazard | Control | Remaining concern |
|---|---|---|
| Missed pneumonia | Radiologist reads every image; triage only reorders | Long queues at night may still delay low-ranked cases |
| Poor performance on another manufacturer's X-ray machine | Input checks; external validation on several machines | New machines bought after approval |
| Automation bias — radiologist trusts the "normal" ranking | Training; no "normal" label shown, only order | Hard to measure without monitoring |

### 8.2 Indirect harm

Software rarely injures anyone directly. India's guidance defines **indirect harm** as injury caused by erroneous, delayed, or misleading information, or a loss of effectiveness — and gives **unintended bias in clinical decisions** caused by software output as an example. This connects the regulatory view of safety to the failure modes you studied in Unit III.

### 8.3 Post-market surveillance

Approval is a snapshot. Unit IV showed that performance can fall at a new site or as data change. **Post-market surveillance (PMS)** is the manufacturer's legal duty to keep watching. CDSCO expects a PMS plan proportionate to the risk class, including:

- complaint handling and **adverse event** reporting;
- tracking of software patches and **cybersecurity** monitoring;
- for AI: **drift monitoring**, error rates, clinical safety signals, and user feedback;
- collection of **real-world evidence** from Indian healthcare settings;
- **field safety corrective actions** — e.g. a correction, update, or recall — when problems are found.

**Why it matters: the Epic Sepsis Model.** A sepsis-warning model supplied with a widely used US electronic health record was used by hundreds of hospitals. When researchers at the University of Michigan validated it independently (Wong et al., 2021, 38,455 hospitalisations), they found an AUC of **0.63**; the model alerted on **18%** of all hospitalised patients yet **missed 67%** of patients who developed sepsis. The developer disputed aspects of the evaluation, including the sepsis definition. The lesson is the same either way: a widely deployed model's performance was not known until a hospital measured it locally. Monitoring is a safety control, not paperwork.

## 9. Case discussion

Return to the Bengaluru startup from Section 1, which wants to launch the chest X-ray triage tool in India first.

1. Write a one-sentence intended-use statement for the triage version. Which words in it determine the risk class?
2. Using Section 4, classify it. What class would it become if marketing materials claimed "no radiologist review needed for normal images"?
3. The model was trained on public paediatric X-rays from one hospital outside India. What evidence would CDSCO expect before licensing, and what study would you design?
4. Could the tool use the US CDS exclusion? Why not?
5. Draft three items of an Algorithm Change Protocol for retraining on Indian data next year.
6. Name two monitoring measures you would report in post-market surveillance, and the threshold that would trigger a field safety corrective action.

## 10. Revision summary

| Concept | Key idea |
|---|---|
| Legal hierarchy | Act → rules (binding) → guidance (expected) → standards (referenced) |
| Medical device software | Defined by intended medical purpose, not by technology; teaching software is excluded |
| Risk classification | Significance (treat/diagnose, drive, inform) × situation (critical, serious, non-serious) → India Class A–D |
| Human in the loop | Removing human review can raise the risk class (B → D in the worked example) |
| India | MDR-2017; CDSCO MDSW guidance (July 2026); Indian-population validation; drift monitoring |
| United States | 510(k), De Novo, PMA; CDS exclusion does not cover image analysis |
| European Union | MDR Rule 11 plus AI Act high-risk duties for AI devices (from August 2028) |
| Changing models | Locked vs adaptive; PCCP (US), ACP (India); GMLP principles |
| Evidence | Clinical association → technical performance → clinical performance → clinical benefit |
| Safety | Hazard → hazardous situation → harm; controls; indirect harm; post-market surveillance |

## 11. Review questions

1. Is each of these a medical device? (a) a symptom checker that tells users whether to visit an emergency department; (b) a hospital bed-booking app; (c) this course's Lab 12 notebook. Justify using intended purpose.
2. A smartphone app used by patients without clinical support estimates whether a skin lesion may be melanoma (a serious skin cancer) and recommends seeing a doctor. Classify it under India's matrix, noting the guidance on non-clinical users.
3. Explain why the same pneumonia model can be Class B or Class D.
4. What is the difference between a guidance document and a rule? Why do companies follow guidance anyway?
5. Recompute the IDx-DR table for a prevalence of 5%. What happens to the PPV, and why?
6. A vendor says its device "is FDA-cleared, so it is proven to improve outcomes". What two questions would you ask?
7. Give one advantage and one risk of a predetermined change plan.
8. Map these to ISO 14971 terms: (a) model outputs wrong patient's risk; (b) the nurse acts on it during a busy shift; (c) the patient receives an unnecessary drug.
9. Why did the Epic Sepsis Model evaluation surprise many hospitals, and which post-market activity would have caught the problem earlier?

## 12. Further reading

**Core sources used in these notes**

- CDSCO. [Guidance Document on Medical Device Software, Doc No. CDSCO/MD/GD/MDSW/01/2026](https://cdsco.gov.in/opencms/export/sites/CDSCO_WEB/Pdf-documents/Guidance-document-on-Medical-Device-Software-under-MDR-2017.pdf), circular dated 21 July 2026 (checked October 2026).
- IMDRF. [Software as a Medical Device: Possible Framework for Risk Categorization and Corresponding Considerations](https://www.imdrf.org/sites/default/files/docs/imdrf/final/technical/imdrf-tech-140918-samd-framework-risk-categorization-141013.pdf) (N12, 2014) and [SaMD: Clinical Evaluation](https://www.imdrf.org/docs/imdrf/final/technical/imdrf-tech-170921-samd-n41-clinical-evaluation_1.pdf) (N41, 2017).
- IMDRF. [Good machine learning practice for medical device development: guiding principles](https://www.imdrf.org/sites/default/files/2025-02/IMDRF_AIML%20WG_GMLP_N88%20Final.pdf) (N88, January 2025).
- FDA. [Clinical Decision Support Software guidance](https://www.fda.gov/regulatory-information/search-fda-guidance-documents/clinical-decision-support-software) (January 2026) and [Predetermined Change Control Plans for AI-enabled device software functions](https://www.fda.gov/regulatory-information/search-fda-guidance-documents/marketing-submission-recommendations-predetermined-change-control-plan-artificial-intelligence) (final guidance first issued December 2024; page updated August 2025).
- European Union. [Regulation (EU) 2024/1689 (AI Act)](https://eur-lex.europa.eu/eli/reg/2024/1689/oj) and [Regulation (EU) 2017/745 (Medical Device Regulation)](https://eur-lex.europa.eu/eli/reg/2017/745/oj). Application dates for high-risk AI were amended by Regulation (EU) 2026/1744 (in force 27 July 2026); check the consolidated text for current dates.

**Evidence examples**

- Abràmoff MD et al. [Pivotal trial of an autonomous AI-based diagnostic system for detection of diabetic retinopathy in primary care offices](https://doi.org/10.1038/s41746-018-0040-6). *npj Digital Medicine* 2018;1:39.
- Lång K et al. [Artificial intelligence-supported screen reading versus standard double reading in the Mammography Screening with Artificial Intelligence trial (MASAI)](https://doi.org/10.1016/S1470-2045(23)00298-X). *Lancet Oncology* 2023;24:936–944; final interval-cancer results in [*The Lancet*, 2026](https://doi.org/10.1016/S0140-6736(25)02464-X).
- Wu E et al. [How medical AI devices are evaluated: limitations and recommendations from an analysis of FDA approvals](https://doi.org/10.1038/s41591-021-01312-x). *Nature Medicine* 2021;27:582–584.
- Wong A et al. [External validation of a widely implemented proprietary sepsis prediction model in hospitalized patients](https://doi.org/10.1001/jamainternmed.2021.2626). *JAMA Internal Medicine* 2021;181(8):1065–1070.

**Optional**

- Collins GS et al. [TRIPOD+AI statement](https://doi.org/10.1136/bmj-2023-078378). *BMJ* 2024;385:e078378.
- Liu X et al. [CONSORT-AI extension](https://doi.org/10.1038/s41591-020-1034-x). *Nature Medicine* 2020;26:1364–1374.

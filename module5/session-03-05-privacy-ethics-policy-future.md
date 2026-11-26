# Sessions 3–5 — Privacy, Ethics, Policy, and the Future of Healthcare AI

**AI in Healthcare · Unit V · B.Tech AI, Third Year**  
**Student lecture notes and primary study material**  
**Previous session:** [Sessions 1–2 — Regulating clinical AI: laws, frameworks, safety and efficacy](session-01-02-regulation-safety-efficacy.md)

## Learning objectives

After studying this chapter, you should be able to:

1. Apply the main duties of India's Digital Personal Data Protection Act and Rules to a healthcare AI project.
2. Explain how the Ayushman Bharat Digital Mission moves health records between providers with the patient's consent.
3. Explain why removing names does not make health data anonymous, and name techniques that reduce privacy risk.
4. Use the ICMR ethical principles to review a healthcare AI project, and explain how the choice of label can create unfairness.
5. Analyse policy challenges to adopting healthcare AI in India and judge emerging trends critically.
6. Combine the course's ideas into a set of questions for judging any healthcare AI claim.

You need Sessions 1–2 of this unit, Unit I Session 6 (FHIR), and Unit IV Session 1 (labels and data requirements). Medical and legal knowledge is not assumed.

## Contents

**Part A — Privacy and data governance**

1. [The problem: can the hospital share its X-rays?](#1-the-problem-can-the-hospital-share-its-x-rays)
2. [Vocabulary: privacy, confidentiality, security, governance](#2-vocabulary-privacy-confidentiality-security-governance)
3. [India's data protection law: the DPDP Act and Rules](#3-indias-data-protection-law-the-dpdp-act-and-rules)
4. [The Ayushman Bharat Digital Mission](#4-the-ayushman-bharat-digital-mission)
5. [De-identification and its limits](#5-de-identification-and-its-limits)

**Part B — Ethics and responsible AI**

6. [From ethical principles to practice](#6-from-ethical-principles-to-practice)
7. [Fairness: when the label is the problem](#7-fairness-when-the-label-is-the-problem)
8. [Ethics review, consent, and documentation](#8-ethics-review-consent-and-documentation)

**Part C — Policy, the future, and synthesis**

9. [Policy challenges in adopting healthcare AI](#9-policy-challenges-in-adopting-healthcare-ai)
10. [Emerging trends and future ecosystems](#10-emerging-trends-and-future-ecosystems)
11. [Course synthesis: hope, not hype](#11-course-synthesis-hope-not-hype)
12. [Revision summary](#12-revision-summary)
13. [Review questions](#13-review-questions)
14. [Further reading](#14-further-reading)

## Part A — Privacy and data governance

## 1. The problem: can the hospital share its X-rays?

Continue the case from Sessions 1–2. The Bengaluru startup's chest X-ray triage tool must be validated on Indian patients. A children's hospital in Pune (fictional) has five years of chest X-rays with radiology reports and diagnoses, and the startup asks for a copy.

The hospital's data protection officer, ethics committee, and doctors must answer:

- **Is it allowed?** Whose permission is needed — each patient's, a parent's, an ethics committee's?
- **What must be removed or protected** before data leave the hospital, and is that enough?
- **Is it ethical?** Will the tool work as well for every group of children, and who benefits?
- **Will anyone use it?** Even a lawful, ethical, approved tool can fail to be adopted.

Part A covers the first two questions, Part B the third, and Part C the fourth, followed by a synthesis of the course.

## 2. Vocabulary: privacy, confidentiality, security, governance

These four terms are often mixed up:

| Term | Meaning | Example |
|---|---|---|
| **Privacy** | A person's right to control information about themselves | A patient decides whether her records may be used to train a model |
| **Confidentiality** | A professional duty not to disclose what a patient shares | A doctor does not discuss a diagnosis with the patient's employer |
| **Security** | Technical and organisational protection against unauthorised access | Encryption, access control, audit logs |
| **Data governance** | The rules, roles, and processes deciding who may use which data, for what, and how this is checked | A data-access committee approves the startup's request with conditions |

Security is necessary but not sufficient: a perfectly secure database can still be used for a purpose the patient never agreed to.

## 3. India's data protection law: the DPDP Act and Rules

### 3.1 The law and its timeline

India's general data protection law is the **Digital Personal Data Protection (DPDP) Act, 2023**, made operational by the **DPDP Rules, 2025**, notified on 13–14 November 2025. The Rules come into force in phases:

| From | What applies |
|---|---|
| 13 November 2025 | Definitions; setting up the **Data Protection Board of India** |
| 13 November 2026 | Registration of **consent managers** |
| 13 May 2027 | The core duties: notices and consent, security, breach reporting, individual rights, children's data, the research exemption |

So in October 2026, most duties are **enacted but not yet in force**. Organisations are expected to use the transition period to prepare.

### 3.2 Who's who

| Role | Meaning | In the X-ray case |
|---|---|---|
| **Data Principal** | The person the data are about (for a child, includes the parent or guardian) | Each child, through a parent |
| **Data Fiduciary** | Decides why and how personal data are processed | The hospital; the startup, for its own purposes |
| **Data Processor** | Processes data on a fiduciary's behalf | A cloud provider storing the images |
| **Consent Manager** | A registered platform where people give, review, and withdraw consent | ABDM's consent manager (Section 4) |

### 3.3 When may personal data be processed?

There are two lawful grounds:

1. **Consent** that is free, specific, informed, unambiguous, and can be withdrawn as easily as it was given, after a clear notice of the purpose.
2. **Certain legitimate uses** listed in Section 7 of the Act, which include responding to a **medical emergency** involving a threat to life or health, and providing **treatment or health services during an epidemic** or other threat to public health.

Treating a patient in an emergency does not wait for a consent form. Training a commercial AI model on years of stored records is a different purpose, and is not an emergency.

### 3.4 Duties and rights that matter for AI

- **Purpose limitation and data minimisation.** Use only the data needed for the stated purpose.
- **Security safeguards**, including encryption, access control, and logs kept for at least one year.
- **Breach reporting.** Affected people must be told without delay; the Board must receive an initial report without delay and a detailed report **within 72 hours**.
- **Children.** Verifiable parental consent is needed. Clinical establishments and healthcare professionals are exempt to the extent needed to provide health services to the child — but not, for example, to build a product.
- **Rights of the person**: to access, correct, update, and erase data, to nominate someone to act for them, and to complain. Organisations must respond within 90 days.
- **Penalties** of up to ₹250 crore for failing to keep reasonable security safeguards.

### 3.5 The research exemption

The Act does **not** apply to processing that is necessary for **research, archiving, or statistical purposes**, provided the data are **not used to take a decision about a specific person** and the processing follows standards in the Rules' Second Schedule (lawful purpose, minimal data, accuracy, limited retention, security, accountability).

For the X-ray case, validating the model on past images may fall under research. Using the tool to decide about a particular child would not. And the exemption covers only *this* law: ethics committee approval (Section 8) and the hospital's confidentiality duties still apply.

### 3.6 One difference from Europe

The EU's General Data Protection Regulation (GDPR) puts health data in a **special category** with stricter conditions. The DPDP Act does not define a separate category of sensitive or health data; all digital personal data are treated alike. Health-specific protection in India therefore comes from other sources too: ABDM policies, ICMR guidelines, and professional confidentiality.

## 4. The Ayushman Bharat Digital Mission

### 4.1 The building blocks

The **Ayushman Bharat Digital Mission (ABDM)**, run by the National Health Authority, is building a national digital health infrastructure. Its registries and services include:

| Building block | What it is |
|---|---|
| **ABHA** (Ayushman Bharat Health Account) | A 14-digit health ID for a person; more than 90 crore ABHA accounts had been created by May 2026 |
| **HPR** (Healthcare Professionals Registry) | Verified list of doctors, nurses, and other professionals |
| **HFR** (Health Facility Registry) | Verified list of hospitals, clinics, laboratories, pharmacies |
| **HIE-CM** (Health Information Exchange and Consent Manager) | Routes requests for records and records the patient's consent |

Recall from Unit I that an **ABHA number is a record identifier**, not a terminology code: it says *who* the record belongs to, not *what* condition it describes. ABDM exchanges records in **FHIR R4** format, the standard you met in Unit I.

### 4.2 How a record moves with consent

ABDM is **federated**: records stay with the hospital or clinic that created them. A record moves only when the patient approves:

```text
Health Information User (HIU)            Patient (PHR app)                Health Information Provider (HIP)
e.g. a new specialist                                                       e.g. the hospital that did the X-ray
     |                                          |                                    |
     |-- 1. consent request (purpose, -------->|                                    |
     |      record types, date range, expiry)   |                                    |
     |                                          |-- 2. patient approves or denies    |
     |                                          |   (via consent manager) ---------->|
     |<------------------- 3. encrypted FHIR records sent directly ------------------|
```

The consent request is specific — purpose, type of record, period, and expiry — and the patient can revoke it. The consent manager records the permission but does not keep the medical content.

### 4.3 What this means for AI

ABDM was designed for **care** — getting a patient's records to the doctor treating them. Using shared records to **train or validate models** is a different purpose. Consent given so that a specialist can read an X-ray does not cover giving that X-ray to a startup. Secondary use for research is governed by ABDM's Health Data Management Policy together with the DPDP Act and ethics review. The policy has been revised since it was first published in 2020, so check the current version on [abdm.gov.in](https://abdm.gov.in/) before relying on its details.

## 5. De-identification and its limits

### 5.1 Terms

- **Identifiers** directly name a person: name, phone number, ABHA number, hospital record number.
- **Quasi-identifiers** do not name a person alone but can in combination: date of birth, pin code, sex, admission date, rare diagnosis.
- **De-identification** removes or alters identifiers. **Pseudonymisation** replaces them with a code, keeping a key that can reverse it.
- **Anonymisation** means no one can reasonably re-identify the person. Data that are truly anonymous are no longer personal data, but true anonymity is hard to achieve.

### 5.2 Why removing names is not enough

A well-known US study (Sweeney, 2000) estimated that about **87%** of the US population could probably be uniquely identified from just three quasi-identifiers: five-digit ZIP code, sex, and date of birth.

A small example. The hospital removes names but keeps these fields:

| Pin code | Sex | Date of birth | Diagnosis |
|---|---|---|---|
| 411001 | F | 2019-03-14 | Pneumonia |
| 411001 | M | 2019-07-02 | Normal |
| 411038 | F | 2020-11-21 | Pneumonia |

A neighbour who knows a girl born on 14 March 2019 in pin code 411001 learns her diagnosis. Coarsening — birth *year* and the first three digits of the pin code — would make each row match many children.

Images carry extra risks: DICOM files contain header fields with names and IDs (Unit III imaging lecture), some images have text burnt into the pixels, and head scans can be used to reconstruct a face.

### 5.3 Techniques that reduce risk

| Technique | Idea | Limitation |
|---|---|---|
| Remove and coarsen identifiers | Delete direct identifiers; generalise quasi-identifiers | Loses detail; rare cases stay identifiable |
| Access control and data-use agreements | Only approved users, for an approved purpose, in a secure environment | Relies on trust and audits |
| **Synthetic data** | Generate artificial records with similar patterns (as in this course's labs) | May miss real patterns or leak rare real cases; cannot prove clinical performance |
| **Federated learning** | The model travels to each hospital; only model updates are shared, not records | Updates can still leak information; harder to check data quality |

Federated learning has been used at scale: 20 institutions on several continents trained a COVID-19 outcome model without pooling patient records (Dayan et al., 2021).

## Part B — Ethics and responsible AI

## 6. From ethical principles to practice

### 6.1 The classical principles

Medical ethics often starts from four principles:

| Principle | Meaning | AI question |
|---|---|---|
| **Autonomy** | Respect people's right to decide for themselves | Do patients know AI is involved, and may they refuse? |
| **Beneficence** | Act for the patient's benefit | Is there evidence that it helps (Sessions 1–2)? |
| **Non-maleficence** | Do no harm | What are its failure modes, and who catches them? |
| **Justice** | Distribute benefits and burdens fairly | Does it work equally well for all groups, and who can access it? |

### 6.2 The ICMR guidelines

The Indian Council of Medical Research (ICMR) published the *Ethical Guidelines for Application of Artificial Intelligence in Biomedical Research and Healthcare* in 2023. They set out ten principles. The table links each to practices from this course:

| ICMR principle | What it asks | Course practice |
|---|---|---|
| 1. Autonomy | Humans keep control of medical decisions; patients may accept or reject AI | Human-in-the-loop, approval interrupts (Unit III) |
| 2. Safety and risk minimisation | Prevent harm, including from misuse and cyberattack | Risk management, failure modes |
| 3. Trustworthiness | AI should be explainable, reliable, and validated | Explainability limits (Unit IV Session 5) |
| 4. Data privacy | Protect personal data throughout | Part A of this chapter |
| 5. Accountability and liability | Clear responsibility, with audits | Audit trails (Unit III Session 6) |
| 6. Optimisation of data quality | Accurate, representative, unbiased data | Data quality checks (Unit IV Session 1) |
| 7. Accessibility, equity, inclusiveness | Benefits reach all, including underserved groups | Deployment setting, cost |
| 8. Collaboration | Work across disciplines and institutions | Stakeholders (Unit I) |
| 9. Non-discrimination and fairness | No unfair bias against groups | Subgroup audits (Lab 13) |
| 10. Validity | Rigorous clinical and lab validation before use | Internal and external validation |

The World Health Organization's 2021 guidance on AI for health sets out six similar principles. Its 2024 guidance applies them to large multimodal models such as the medical foundation models you met in Unit II.

### 6.3 Principles conflict

Principles often pull in different directions. For example:

- More **data sharing** improves validity and fairness, but increases privacy risk.
- A more **accurate** but opaque model may be less trustworthy to clinicians than a simpler one.
- Deploying only after an RCT protects safety but **delays access** to a possibly helpful tool.

Ethics does not remove these trade-offs. It requires you to state them, justify the choice, and let affected people be heard.

## 7. Fairness: when the label is the problem

A widely used US algorithm chose patients for an extra care-management programme. It predicted each patient's **future healthcare cost** and used that as a stand-in for **health need**. Obermeyer and colleagues (2019) found that, at the same risk score, Black patients were considerably sicker than white patients. Less money had historically been spent on Black patients with the same needs, so cost underestimated their need.

The researchers showed that correcting the bias would raise the proportion of Black patients selected for extra help from **17.7% to 46.5%**. The developer then worked with them to reduce the bias.

Three lessons:

1. The model was **accurate at predicting its label**. The unfairness came from choosing a label that differed from the real target (Unit IV Session 1).
2. **The model did not use race as an input**, yet it was still biased: cost carried the history of unequal spending. Leaving out a sensitive attribute does not guarantee fairness.
3. **Subgroup audits** — comparing outcomes for groups at the same predicted score — revealed the problem. That is the audit you performed in Lab 13.

For the X-ray tool, ask: were children from all regions, X-ray machine types, and economic backgrounds represented? Is "pneumonia" defined by an expert reading, or by a billing code?

## 8. Ethics review, consent, and documentation

### 8.1 Ethics committee review

Under the ICMR guidelines, research involving human participants or their data needs approval from an **Institutional Ethics Committee (IEC)**. Key points for AI:

- **IEC approval is required** even when only **retrospective data** are used to develop and validate an AI tool.
- The researcher may ask the committee for a **waiver of consent** when the research involves less than minimal risk and the waiver will not harm participants' rights and welfare — common when contacting thousands of past patients is impractical.
- When consent is sought, participants must understand the alternatives to the AI tool, the role of humans versus technology, and the possible errors at each step.

### 8.2 Documentation

Responsible AI depends on writing things down so that others can check them:

- **Datasheets for datasets** (Gebru et al.) record how data were collected, who is represented, and known gaps.
- **Model cards** (Mitchell et al.) record intended use, performance by subgroup, and limitations.
- The regulatory documents of Sessions 1–2 — intended use statement, risk file, change protocol — serve the same purpose for authorities.

## Part C — Policy, the future, and synthesis

## 9. Policy challenges in adopting healthcare AI

A tool can pass every regulatory and ethical test and still never reach patients. Common barriers:

| Challenge | Example in the Indian context | Possible policy response |
|---|---|---|
| **Fragmented data** | Records on paper or in unconnected systems | ABDM registries and FHIR-based exchange |
| **Evidence from elsewhere** | Models trained on foreign data | CDSCO expectation of validation in representative Indian populations |
| **Unclear liability** | Who is responsible when an AI-assisted decision harms a patient? | Clear intended use, human review, audit trails; law still developing |
| **Who pays** | No payment code for AI-assisted reading | Public procurement and insurance schemes that require evidence |
| **Infrastructure and access** | Unreliable connectivity, few radiologists in rural areas | Offline-capable tools; deployment where the gap is largest |
| **Language and literacy** | Many languages; varied health and digital literacy | Patient-facing tools tested in local languages |
| **Skills and regulatory capacity** | Few staff who understand both AI and medicine | Training (Unit IV Session 5); regulatory guidance |

India has chosen not to pass a single AI law. MeitY's *India AI Governance Guidelines* (November 2025) are voluntary and favour **sector-specific** rules: for healthcare, that means CDSCO for devices, DPDP and ABDM for data, and ICMR for research ethics.

## 10. Emerging trends and future ecosystems

Each trend below offers real promise, and each raises a question you can already answer with this course's tools.

| Trend | Promise | Question to ask |
|---|---|---|
| **Medical foundation and multimodal models** (Unit II) | One model for many tasks: reports, images, questions | Was it evaluated on *this* narrow task, against a baseline, with hallucinations counted? |
| **AI agents** (Unit III) | Complete multi-step administrative tasks | Which actions can it take without approval, and what happens with injected instructions? |
| **Ambient AI scribes** (Unit III) | Less documentation time for clinicians | Who checks the note, and are errors found before they enter the record? |
| **Remote monitoring and wearables** | Care outside the hospital | Who responds to alerts, and how many are false? |
| **Federated and privacy-preserving learning** | Train on data from many hospitals without pooling | Is data quality checked at each site? |
| **Continuous monitoring after deployment** | Detect drift early | Who owns the tool and has authority to switch it off? |

A **future healthcare AI ecosystem** links these: interoperable records under the patient's control (ABDM), models validated on local populations (CDSCO), ethical oversight (ICMR), data protection (DPDP), and monitoring that continues after launch.

## 11. Course synthesis: hope, not hype

The syllabus title for this topic echoes the US National Academy of Medicine's 2019 report *Artificial Intelligence in Health Care: The Hope, the Hype, the Promise, the Peril*. Each unit of this course added one set of questions:

| Unit | Core question |
|---|---|
| I — Foundations | What data exist, in which form, and who is involved? |
| II — Applications | What clinical problem is solved, for whom, and at what decision point? |
| III — Risks and human-centred AI | How does it fail, and how do people interact with it? |
| IV — Development, validation, deployment | Does it work on new patients, and does it fit the workflow? |
| V — Regulation, policy, future | Is it lawful, safe, ethical, and sustainable? |

**Ten questions for any healthcare AI claim:**

1. What is the **intended use**: task, user, patients, setting, decision time?
2. What is the **label**, and does it measure the real target?
3. What **evidence** exists: retrospective, external, prospective, randomised?
4. How does it perform on **people like ours**, and in each subgroup?
5. Is it **calibrated**, and what threshold is used, with how many false alarms?
6. What are its **failure modes**, and who catches them?
7. Where does it sit in the **workflow**, and can people override it?
8. Is the **data use** lawful and consented, and is privacy protected?
9. What is its **regulatory status** and risk class?
10. Who **monitors** it after launch, and what would make us stop?

**Hope** means a clear problem, a narrow intended use, strong evidence, and honest limitations. **Hype** means a broad promise, accuracy from one dataset, and no plan for failure. The same model can be either, depending on how it is evaluated, deployed, and governed.

## 12. Revision summary

| Concept | Key idea |
|---|---|
| Privacy vs security | Security protects data; privacy and governance decide whether a use is allowed |
| DPDP Act and Rules | Consent or legitimate use; 72-hour breach report to the Board; core duties from 13 May 2027 |
| Research exemption | Applies when data are not used for decisions about a specific person and standards are met |
| ABDM | ABHA, HPR, HFR, consent manager; federated, consent-based FHIR exchange |
| De-identification | Quasi-identifiers can re-identify; coarsen, control access, use synthetic data or federated learning |
| ICMR principles | Ten principles, from autonomy to validity; IEC approval needed even for retrospective data |
| Fairness | A biased label can make an accurate model unfair; subgroup audits reveal it |
| Policy | Data, evidence, liability, payment, access, language, skills; India uses sector-specific rules |
| Synthesis | Ten questions: intended use to monitoring |

## 13. Review questions

1. A hospital encrypts all its data but sells de-identified records to an insurer without telling patients. Which of the four terms in Section 2 does this violate, and which does it respect?
2. Under the DPDP Act, may a doctor share an unconscious patient's records with another hospital without consent? Which provision applies?
3. Explain why the research exemption would not cover using the X-ray tool to decide whether a specific child is admitted.
4. In ABDM, where are a patient's records stored, and what does the consent manager hold?
5. Suggest two changes to the table in Section 5.2 that reduce re-identification risk. What analysis might they prevent?
6. Choose three ICMR principles and say how each applies to the X-ray triage tool.
7. In the Obermeyer case, why was the algorithm biased even though it did not use race as an input? What label would you choose instead?
8. A student team wants to use retrospective hospital data for their capstone project. What approval is needed, and when might consent be waived?
9. Pick one emerging trend from Section 10 and write two evaluation steps you would require before it is used in an Indian district hospital.
10. Apply the ten questions in Section 11 to the heart-disease referral model from Unit IV. Which questions can you answer, and which remain open?

## 14. Further reading

**Indian law and policy (checked October 2026)**

- Ministry of Electronics and Information Technology. [Digital Personal Data Protection Act, 2023](https://www.meity.gov.in/static/uploads/2024/06/2bf1f0e9f04e6fb4f8fef35e82c42aa5.pdf) and [Digital Personal Data Protection Rules, 2025](https://www.meity.gov.in/static/uploads/2025/11/53450e6e5dc0bfa85ebd78686cadad39.pdf) (G.S.R. 846(E), 13 November 2025).
- Press Information Bureau. [DPDP Rules, 2025 notified — explainer](https://static.pib.gov.in/WriteReadData/specificdocs/documents/2025/nov/doc20251117695301.pdf), 17 November 2025.
- National Health Authority. [Ayushman Bharat Digital Mission](https://abdm.gov.in/) — building blocks and Health Data Management Policy.
- ICMR. [Ethical Guidelines for Application of Artificial Intelligence in Biomedical Research and Healthcare](https://www.icmr.gov.in/ethical-guidelines-for-application-of-artificial-intelligence-in-biomedical-research-and-healthcare) (2023); also archived on [Zenodo](https://zenodo.org/records/8262489).
- MeitY. *India AI Governance Guidelines* (November 2025), available from the IndiaAI and MeitY websites.

**International guidance**

- World Health Organization. [Ethics and governance of artificial intelligence for health](https://www.who.int/publications/i/item/9789240029200) (2021) and its [guidance on large multi-modal models](https://www.who.int/publications/i/item/9789240084759) (2024).
- Matheny M et al., eds. [Artificial Intelligence in Health Care: The Hope, the Hype, the Promise, the Peril](https://nam.edu/artificial-intelligence-special-publication/). National Academy of Medicine, 2019.

**Studies cited**

- Obermeyer Z et al. [Dissecting racial bias in an algorithm used to manage the health of populations](https://doi.org/10.1126/science.aax2342). *Science* 2019;366:447–453.
- Sweeney L. [Simple demographics often identify people uniquely](https://dataprivacylab.org/projects/identifiability/paper1.pdf). Carnegie Mellon University, Data Privacy Working Paper 3, 2000.
- Dayan I et al. [Federated learning for predicting clinical outcomes in patients with COVID-19](https://doi.org/10.1038/s41591-021-01506-3). *Nature Medicine* 2021;27:1735–1743.
- Mitchell M et al. [Model cards for model reporting](https://doi.org/10.1145/3287560.3287596). FAT* 2019.
- Gebru T et al. [Datasheets for datasets](https://doi.org/10.1145/3458723). *Communications of the ACM* 2021;64(12):86–92.

# Session 7 — Healthcare AI: Capabilities, Stakeholders, and Responsible Adoption

**AI in Healthcare · Unit I · B.Tech AI, Third Year**  
**Student lecture notes and primary study material**  
**Companion practical:** [Lab 1 — Healthcare Data Ecosystem](../labs/lab-01-data-ecosystem.ipynb)

## Learning objectives

By the end of this lecture, you should be able to:

1. Distinguish AI, machine learning, deep learning, and rule-based systems.
2. Match a healthcare problem to an AI capability and identify its data requirements.
3. Explain the roles and competing needs of healthcare AI stakeholders.
4. Distinguish technical performance from useful adoption and explain trust, equity, and inclusion.

You need basic knowledge of AI/ML and the concepts from the [Healthcare Data Ecosystem lecture](session-06-data-ecosystem.md). Medical knowledge is not assumed. The patient scenarios and numerical examples are fictional.

## Contents

1. [What do we mean by healthcare AI?](#1-what-do-we-mean-by-healthcare-ai)
2. [Capabilities and data dependence](#2-capabilities-and-data-dependence)
3. [Stakeholders and workflows](#3-stakeholders-and-workflows)
4. [Adoption, trust, equity, and challenges](#4-adoption-trust-equity-and-challenges)
5. [Case discussion: moving to another hospital](#5-case-discussion-moving-to-another-hospital)
6. [Revision summary](#6-revision-summary)
7. [Review questions](#7-review-questions)
8. [Further reading and lab connection](#8-further-reading-and-lab-connection)

## 1. What do we mean by healthcare AI?

### 1.1 A patient-care problem

A hospital wants to help its ward team identify patients whose condition may worsen soon. **Patient deterioration** means worsening health that may require urgent assessment or more intensive care. Staff already review patients; the proposed system would help prioritize attention.

Our fictional system reads recent measurements and produces a risk estimate. Before choosing an algorithm, ask: **What event are we predicting, who receives the result, and what can they do with it?** All numbers and situations in this running example are invented teaching examples, not clinical recommendations.

### 1.2 AI, machine learning, and deep learning

| Term | Meaning | Healthcare example |
|---|---|---|
| **Artificial intelligence (AI)** | The broad field of building systems that perform tasks such as reasoning, perception, prediction, and language processing | A system that helps prioritize patient review |
| **Machine learning (ML)** | An approach within AI that learns patterns from data rather than having every decision rule specified manually | Learning an association between recorded measurements and later deterioration |
| **Deep learning (DL)** | An approach within ML using neural networks with multiple layers to learn representations | Learning visual patterns from medical images |

The relationship is **DL ⊂ ML ⊂ AI**. A neural network is a model made of connected computational units whose parameters are adjusted during learning. “Deep” refers to its layers, not a guarantee of deeper medical understanding.

A **rule-based system** applies explicitly written conditions. For example, a programmer can specify that an alert should trigger when a measurement crosses a threshold. The system follows that condition; it does not learn the threshold from patient examples.

A **learned model** estimates a relationship from data. During development, it might learn how several measurements together relate to later deterioration. When given a new patient's measurements, it applies that learned relationship to estimate risk. Simple threshold alerts are conventional decision support; not every automated rule is usefully described as AI.

Rules can be transparent but brittle; learned models can capture more complex patterns but inherit limitations of their training data. A more complex model is not automatically the better choice. Start with the task and a simple baseline.

### 1.3 Similar methods, different settings

| Outside healthcare | Related healthcare task | Question to add in healthcare |
|---|---|---|
| Detecting unusual financial transactions | Detecting unusual patient measurements | Is this a real clinical change or a sensor error? |
| Finding defects in product images | Detecting a finding in a medical image | Which patients or imaging devices were represented in evaluation? |
| Summarizing business documents | Drafting a clinical-note summary | Has a negation, medication detail, or uncertainty been changed? |

The computational method may transfer, but the target, data, consequences of errors, and review process must be reconsidered for each setting. Other industries can also be high stakes; the healthcare context requires its own evidence.

## 2. Capabilities and data dependence

### 2.1 What can a system do?

| Capability | Typical output | Example |
|---|---|---|
| Classification | A category or category probability | Estimate whether deterioration occurs within a defined window |
| Numerical prediction | A number | Estimate length of hospital stay |
| Detection or segmentation | A location or outlined region | Mark a suspicious region in an image |
| Language extraction | Structured information from text | Extract a symptom and whether it is present or negated |
| Generation | New text or other content | Draft a summary for review |
| Optimization | An allocation or schedule | Assign appointments subject to staffing constraints |

These capabilities can overlap: predicting a future yes/no event is also classification. Optimization can use ML predictions, but need not involve ML itself.

A **generative model** produces content. A **large language model (LLM)** is a language model trained at scale that can perform tasks such as drafting and summarization. Fluent output can contain unsupported statements, often called **hallucinations**. A generated explanation is not evidence that its claims are correct. The World Health Organization's (WHO) guidance on large multimodal models discusses these reliability concerns in healthcare. [WHO guidance, 2025](https://www.who.int/publications/i/item/9789240084759).

### 2.2 From a clinical problem to a prediction task

A clinical goal describes the benefit we want, such as earlier assessment of patients who need help. A computational task specifies what the system will estimate from its inputs. To turn “predict deterioration” into a task, we must define the population, event, and time window:

| Design choice | Fictional specification |
|---|---|
| Population and setting | Adult patients on a hospital ward |
| Prediction time | 09:00 each day |
| Unit of prediction | One eligible patient at that prediction time |
| Inputs | Measurements and history available to the system by 09:00 |
| Target label | Whether an unplanned transfer to intensive care occurs during the next six hours |
| Output | An estimated probability of that recorded event |
| Intended user and action | Ward team reviews flagged patients using clinical judgment |

An **intensive care unit (ICU)** provides care for patients needing intensive monitoring or treatment. ICU transfer is a measurable label, but only a **proxy** for deterioration: bed availability and local practice can affect who is transferred. The model learns the recorded target, which may not fully represent the clinical need.

A risk estimate predicts an outcome under the patterns represented in the data. It does not establish which treatment will prevent that outcome.

### 2.3 The model depends on what the data actually records

| Data issue | Effect on the example | Connection to Lab 1 |
|---|---|---|
| Missing measurements | Absence may reflect care processes rather than normal health | O003 has no numeric result; zero would invent a measurement |
| Inconsistent units | Identical physiological values may appear numerically different | 37 °C and 98.6 °F require conversion before comparison |
| Incorrect linkage | Another visit's information may be assigned to this prediction | Joining notes only by patient misassigns visits |
| Unrepresentative data | Performance may change in another patient population or hospital | A small synthetic fixture cannot demonstrate generalization |

**Temporal leakage** occurs when training or evaluation uses information unavailable at the intended prediction time. For the 09:00 prediction, a result released at 09:20 cannot be an input, even if its sample was collected at 08:45. Future information may define the outcome label; it must not leak into the inputs.

**Training** fits the model using examples: input measurements are paired with the recorded outcome, or **label**. **Inference** applies the fitted model to new inputs whose future outcome is not yet known.

Evaluation should reflect the intended use. To evaluate performance on new patients, keep each patient's records entirely within either the training or test set. Otherwise, familiarity with the same patients can make the test misleading. Changes over time also require evaluation on appropriately later data.

## 3. Stakeholders and workflows

A **stakeholder** is someone who uses, is affected by, builds, funds, or oversees the system. The person receiving the output may differ from the person bearing the consequences.

### 3.1 Who needs what?

| Stakeholder | Main concern | Question for the deterioration system |
|---|---|---|
| Patients and families | Safe care, privacy, understandable communication | How will the alert affect care, and how can concerns be raised? |
| Nurses and doctors | Timely, actionable information | Which patient needs review, and how urgent is it? |
| Hospital administrators | Staffing, service quality, cost | Can the ward respond to the additional reviews? |
| Developers and data teams | Reliable data, evaluation, maintenance | Does the live input pipeline match the tested system? |
| Payers and public-health authorities | Value, access, population outcomes | Are benefits distributed fairly and worth the resources? |
| Regulators and institutional governance teams | Evidence, appropriate use, accountability | What use was evaluated, and who investigates failures? |

These goals can conflict. Increasing the number of alerts may detect more events but also increase workload. Improving an average score may hide worse results for a smaller group. A design decision therefore needs both technical and stakeholder reasoning.

### 3.2 A model is one part of a service

```text
Recorded data → availability/quality checks → risk estimate
                                               |
                                               v
                                    alert sent to ward team
                                               |
                                               v
                                  review → action → outcome

         Monitoring checks data, alerts, responses, and outcomes.
```

The workflow must specify who receives the alert, what information they see, what happens if nobody responds, and how problems are reported. **Human oversight** requires time, authority, and enough context to review the output; adding a “clinician review” label alone does not provide those conditions.

**Automation bias** is over-reliance on a system's output. **Alert fatigue** occurs when frequent or low-value alerts reduce attention. Both can undermine a technically capable system.

### 3.3 Worked example: the cost of alerts

Suppose the system flags 20 patient-time predictions. In this fictional evaluation, 5 are followed by the target event and 15 are not.

```text
Positive predictive value = target events among flagged predictions / all flagged predictions
                          = 5 / 20 = 25%
```

**Positive predictive value (PPV)** describes how often a positive prediction corresponds to the target event. The team reviews 20 alerts to identify these 5 events. This alone does not tell us how many events the system missed: we also need outcomes among unflagged patients.

Whether this workload is worthwhile depends on the consequences of missed events, review cost, and available staff. In live use, actions triggered by alerts may also change outcomes, complicating evaluation.

## 4. Adoption, trust, equity, and challenges

### 4.1 What does “AI is being adopted” mean?

**Adoption** means incorporating a system into actual work. Building a model is only one step toward adoption. An application can be at different stages:

| Stage | What it tells us |
|---|---|
| Research prototype | The idea has been implemented and tested under specified conditions |
| Limited pilot | The system is being assessed in a restricted operational setting |
| Routine use | The system is part of an ongoing workflow and requires maintenance and monitoring |

These stages describe use, not a guarantee of benefit. Even a system in routine use needs evidence about its effects.

AI-enabled medical products already exist: the United States Food and Drug Administration (FDA) maintains a list of devices authorized for marketing in the US. The list is not comprehensive and is not a measure of how widely hospitals use those devices. US authorization also does not establish authorization in India. [FDA AI-enabled medical devices](https://www.fda.gov/medical-devices/software-medical-device-samd/artificial-intelligence-enabled-medical-devices) (checked 7 September 2026).

For any adoption claim, ask whether it reports **availability, actual use, or demonstrated benefit**. A demonstration or strong retrospective test does not by itself show improved care in a working hospital.

For the deterioration system, evaluation must extend from previously collected records to the local workflow: are inputs reliable, can staff respond, and does care improve? Monitoring remains necessary after introduction because both the data and the working environment can change.

### 4.2 Trust, equity, and inclusion

**Trust** should be supported by evidence about when a system works and fails. Users need a clear intended use, understandable limitations, a way to question outputs, and accountable support. An attractive explanation cannot compensate for an unreliable prediction.

**Equity** concerns fair opportunities for benefit and avoidance of unfair burdens. A good overall result can hide poor detection for a smaller patient group. Relevant checks include subgroup errors and differences in access to the service; small samples also limit what can be concluded.

**Inclusion** means involving affected people in design and evaluation. For example, an interface requiring English fluency may exclude some users even if the underlying model is accurate. Patients, nursing staff, and people with accessibility needs can identify problems absent from a benchmark dataset.

WHO's health AI guidance connects these concerns to autonomy, safety, transparency, accountability, inclusion, and equity. [WHO ethics and governance guidance, 2021](https://www.who.int/publications/i/item/9789240029200).

### 4.3 Pair opportunities with conditions for success

| Opportunity | Challenge to address |
|---|---|
| Earlier prioritization of patient review | Missed events, false alerts, and capacity to respond |
| Less repetitive documentation | Incorrect or omitted facts in generated drafts |
| More consistent handling of large datasets | Poor labels, missingness, and changes in data over time |
| Broader access to support | Language, infrastructure, affordability, and population coverage |

**Dataset shift** means the data encountered in use differs from the data used for development. A change in measurement equipment, hospital practice, or patient mix can require renewed evaluation. Privacy also affects design: access should be appropriate to the task, and identifiable records must not be sent to an external tool without the required authorization and governance.

The running example is useful only if appropriate data reaches the model, the predictions support a feasible action, and evaluation shows benefits relative to the existing workflow.

## 5. Case discussion: moving to another hospital

The deterioration system performed well at Hospital A. Hospital B measures patients less frequently, has fewer staff available to review alerts, and serves a different patient population.

Consider the following questions:

1. Which data differences could affect the predictions at Hospital B?
2. Whom should the development team consult before introducing alerts, and why?
3. What evidence, beyond an overall model score, would help determine whether the system is useful?
4. Why might using the same model and alert threshold produce different results at the two hospitals?

The case connects three questions that must be considered together: **Does the data support the task? Can people act on the output? Does that action improve care?**

## 6. Revision summary

| Concept | Key idea |
|---|---|
| AI, ML, and DL | ML learns patterns from data; DL is a neural-network approach within ML; AI is the broader field |
| Task definition | Specify the population, inputs, target, prediction time, and intended action |
| Data dependence | Predictions reflect data quality, labels, availability, and population coverage |
| Stakeholders | Patients, care teams, developers, and institutions have different needs and responsibilities |
| Clinical usefulness | A prediction must support a feasible workflow and beneficial action |
| Responsible adoption | Evaluate reliability, access, workload, and outcomes, and continue monitoring |

## 7. Review questions

1. How does a learned deterioration model differ from a manually specified alert rule?
2. Why might ICU transfer be an imperfect label for patient deterioration?
3. Can an 08:45 sample with a 09:20 result be used for a 09:00 prediction? Explain.
4. If 5 of 20 alerts correspond to events, what can and cannot be concluded?
5. Give an example of an inclusion problem that model accuracy alone would not reveal.

## 8. Further reading and lab connection

- [WHO: Ethics and governance of AI for health](https://www.who.int/publications/i/item/9789240029200) — broader discussion of responsible development and use.
- [WHO: Guidance on large multimodal models](https://www.who.int/publications/i/item/9789240084759) — generative AI opportunities and limitations.
- [FDA: AI-enabled medical devices](https://www.fda.gov/medical-devices/software-medical-device-samd/artificial-intelligence-enabled-medical-devices) — examples of authorized US products; read the list's scope limitations.
- Return to [Lab 1](../labs/lab-01-data-ecosystem.ipynb) and identify how a missing value, incorrect join, or late measurement could affect the fictional prediction workflow. The lab fixture illustrates these issues; it is not suitable for training or evaluating this deterioration model.

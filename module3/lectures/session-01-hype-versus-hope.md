# Session 1 — Hype versus Hope: Judging Healthcare AI Claims

**AI in Healthcare · Unit III · B.Tech AI, Third Year**  
**Student lecture notes and primary study material**  
**Next session:** [Session 2 — Medical Imaging AI](medical_imaging/session-02-lumbar-mri-dicom-medgemma.ipynb), then [Session 3 — Agentic AI in Healthcare](session-03-agentic-ai-in-healthcare.md)

## Learning objectives

By the end of this lecture, you should be able to:

1. Place a healthcare AI claim on an **evidence ladder**, from benchmark accuracy to patient outcomes.
2. Use four real cases to explain why good model accuracy does not guarantee clinical benefit.
3. Define sensitivity (recall), specificity, positive predictive value (PPV, precision) and negative predictive value (NPV), and calculate how disease prevalence changes PPV and NPV.
4. Describe where healthcare AI tends to help and where it tends to fail.

**Prerequisites:** [Unit I session 7](../../module1/lectures/session-07-ai-overview.md) (stakeholders, PPV, alert cost) and basic classification metrics (sensitivity, specificity, AUC).

## Contents

1. [The problem: a headline](#1-the-problem-a-headline)
2. [The evidence ladder](#2-the-evidence-ladder)
3. [Four cases](#3-four-cases)
4. [Why prevalence matters](#4-why-prevalence-matters)
5. [Benefits and limitations: a pattern](#5-benefits-and-limitations-a-pattern)
6. [From predicting to acting](#6-from-predicting-to-acting)
7. [Summary](#7-summary)
8. [Review questions](#8-review-questions)
9. [References](#9-references)

## 1. The problem: a headline

Imagine a news headline: *"AI detects disease X better than doctors."* A hospital is considering buying the product. What should it ask before deciding?

- **Hype** is a claim that runs ahead of its evidence.
- **Hope** is a realistic expectation backed by evidence for a specific task, setting and group of patients.

The difference is rarely about the algorithm. It is about **what was measured, where, and on whom**.

## 2. The evidence ladder

A claim about a healthcare AI system can rest on very different kinds of evidence. Each rung answers a harder question than the one below it.

| Rung | Question it answers | Typical evidence |
|---|---|---|
| 1. Retrospective accuracy | Does it work on a curated, historical dataset? | AUC, sensitivity, specificity on a held-out test set |
| 2. External validation | Does it work at another hospital, on other patients? | The same metrics on data from a different site or time |
| 3. Prospective or silent deployment | Does it work on live data, as cases arrive? | Predictions recorded in real time, often without being shown to staff |
| 4. Trial of outcomes | Do patients do better when it is used? | Randomised clinical trial (RCT) with a clinical outcome |
| 5. Implementation and value | Does it fit the workflow, and is it worth the cost? | Uptake, workload, cost, equity, long-term monitoring |

**Hype usually means treating rung 1 as if it were rung 4.** Two systematic reviews show how common this is:

- Of 81 non-randomised studies comparing deep learning with clinicians in medical imaging, only 9 were prospective and 6 were tested in a real clinical setting. Yet 61 stated in their abstract that the AI was at least comparable to clinicians (Nagendran et al., 2020).
- A review of randomised trials of machine-learning interventions found only 41 RCTs up to October 2021, many at single sites, and none fully followed the CONSORT-AI reporting guideline (Plana et al., 2022).

A system can be useful without a trial: a scheduling tool rarely needs one. But the **higher the stakes of the decision, the higher the rung** the evidence should reach.

## 3. Four cases

### 3.1 A widely used sepsis model, checked independently

**Sepsis** is a life-threatening response to infection, where early antibiotics matter. A commercial electronic health record vendor offered a proprietary sepsis prediction model, the Epic Sepsis Model, which was used at hundreds of US hospitals.

An independent team at Michigan Medicine evaluated it on 38,455 hospitalisations (Wong et al., 2021):

| Finding | Value |
|---|---|
| Hospitalisation-level AUC | 0.63 (95% CI 0.62–0.64) |
| Sepsis cases not identified | 1,709 of 2,552 (67%) |
| Hospitalisations generating an alert (score ≥ 6) | 6,971 of 38,455 (18%) |

**Lesson:** wide adoption is not evidence. The model was in use before rung 2 had been reached at this site, and the evaluation showed both missed cases and a heavy alert burden.

### 3.2 Autonomous diabetic retinopathy screening

**Diabetic retinopathy** is damage to the retina caused by diabetes, a leading cause of preventable vision loss. Screening needs retinal photographs read by a specialist, and many patients miss yearly eye checks.

IDx-DR was tested in a **pre-registered pivotal trial** that enrolled 900 people with diabetes at 10 US primary care sites (819 could be fully analysed). It was compared with a strict reference standard from a reading centre (Abràmoff et al., 2018):

| Measure | Result | Pre-set target |
|---|---|---|
| Sensitivity | 87.2% (95% CI 81.8–91.2) | > 85% |
| Specificity | 90.7% (95% CI 88.3–92.7) | > 82.5% |

In April 2018 the US Food and Drug Administration (FDA) authorised it through the De Novo pathway as the first autonomous AI diagnostic system: it gives a screening result without a clinician reading the image.

**Lesson:** this is **hope done properly**: a narrow task, a defined user and setting, targets fixed in advance, and a prospective trial. Note what the trial did *not* show: whether patients' vision was better protected over time (rung 4).

### 3.3 An accurate model meets real clinics

Google Health researchers studied a deep-learning retinopathy system deployed in **11 clinics in Thailand**, observing nurses and patients over eight months (Beede et al., 2020; Beede, 2020). The model was accurate in testing, but in the clinics:

- many photographs taken in imperfect rooms and lighting did not meet the model's **image-quality threshold**, so patients could not get a result
- slow or unreliable **internet connections** delayed results that were meant to be immediate (as summarised by Okolo, 2020)
- the system changed **nurses' workflow and patients' experience**: an ungradable image could lead to a referral, so patients had to take time off work to attend a clinic further from home. The team later changed the protocol so that specialists reviewed ungradable images before referring

**Lesson:** accuracy is a property of the model and its test data; **usefulness is a property of the whole service**. This is rung 5.

### 3.4 A broad promise without matching evidence

IBM promoted Watson for Oncology to suggest cancer treatments to hospitals worldwide. In 2018, STAT News reported internal IBM documents describing "multiple examples of unsafe and incorrect treatment recommendations". It also reported that the system had been trained largely on hypothetical cases and the preferences of a few specialists, not on guidelines or outcome evidence (Ross and Swetlitz, 2018). In 2022 IBM sold its Watson Health data and analytics assets to an investment firm (Healthcare IT News, 2022).

**Lesson:** the broader the task ("recommend cancer treatment"), the harder it is to produce evidence, and the greater the gap between marketing and rungs 2–4. *This case rests on investigative journalism and company documents, not a peer-reviewed evaluation; treat it as a cautionary example.*

### 3.5 Comparing the cases

| Case | Task breadth | Highest rung reached (in the source cited) | Main lesson |
|---|---|---|---|
| Sepsis model | Narrow | 2: independent validation, which went poorly | Adoption ≠ evidence |
| IDx-DR | Narrow | 3: prospective pivotal trial | Hope done properly |
| Thai clinics | Narrow | 5: implementation study | Accuracy ≠ usefulness |
| Watson for Oncology | Very broad | Unclear | Scope ran ahead of evidence |

## 4. Why prevalence matters

Headlines quote sensitivity and specificity. A clinician receiving a positive result wants the **positive predictive value (PPV)**: the chance that a positive result is a true case. PPV depends on **prevalence**, the fraction of people tested who have the condition.

### 4.1 Four outcomes of a test

Every person tested has the condition or not, and gets a positive or negative result. That gives four outcomes, usually set out as a **confusion matrix**:

| | Has the condition | Does not have the condition |
|---|---|---|
| **Test positive** | True positive (**TP**) | False positive (**FP**): a false alarm |
| **Test negative** | False negative (**FN**): a missed case | True negative (**TN**) |

### 4.2 Four metrics from the same table

Each metric divides one cell by a row total or a column total. Clinical and machine-learning (ML) papers often use different names for the same metric.

| Metric | Other names | Formula | Question it answers |
|---|---|---|---|
| **Sensitivity** | **Recall**, true positive rate | TP / (TP + FN) | Of the people *with* the condition, what fraction test positive? |
| **Specificity** | True negative rate | TN / (TN + FP) | Of the people *without* the condition, what fraction test negative? |
| **Positive predictive value (PPV)** | **Precision** | TP / (TP + FP) | Of the people who test *positive*, what fraction have the condition? |
| **Negative predictive value (NPV)** | — | TN / (TN + FN) | Of the people who test *negative*, what fraction are free of the condition? |

The table has two pairs:

- **Sensitivity and specificity** read *down a column*. Each uses only people with the condition, or only people without it, so the mix of sick and healthy people does not enter the calculation. They describe the **test**.
- **PPV and NPV** read *across a row*. Each mixes people with and without the condition who received the same result, so they depend on how many of each were tested. They describe what a **result means** for the patient in front of you.

Sensitivity is not PPV. A test that flags everyone has 100% sensitivity, but its PPV equals the prevalence.

### 4.3 Worked example: the same test in two settings

Take a test with 90% sensitivity and 90% specificity, used on 10,000 people:

| Setting | Prevalence | With condition | True positives (90%) | Without condition | False positives (10%) | PPV |
|---|---:|---:|---:|---:|---:|---:|
| Specialist clinic | 20% | 2,000 | 1,800 | 8,000 | 800 | 1,800 / 2,600 = **69%** |
| General population | 1% | 100 | 90 | 9,900 | 990 | 90 / 1,080 = **8%** |

$$\text{PPV} = \frac{TP}{TP + FP}$$

The model is identical in both rows. In the general population, **more than 9 of every 10 alerts are false alarms**, because healthy people vastly outnumber sick ones. A model validated in a specialist clinic and then moved to general screening can look accurate on paper and still overwhelm staff with false positives.

The negative results tell the opposite story. The missed cases (FN) are the 10% of people with the condition who test negative, and the true negatives (TN) are the 90% of people without it who test negative:

| Setting | False negatives | True negatives | NPV = TN / (TN + FN) |
|---|---:|---:|---:|
| Specialist clinic (20%) | 200 | 7,200 | 7,200 / 7,400 = **97.3%** |
| General population (1%) | 10 | 8,910 | 8,910 / 8,920 = **99.9%** |

As prevalence falls, PPV falls and NPV rises. In low-prevalence screening, a negative result is very reassuring, while a positive result usually needs a confirmatory test.

### 4.4 How PPV is linked to prevalence

Write prevalence as *p*, the fraction of the people tested who have the condition. Out of *N* people:

- *pN* have the condition. A fraction equal to the sensitivity of them test positive, so TP = sensitivity × *pN*.
- (1 − *p*)*N* do not. A fraction equal to (1 − specificity) of them test positive, so FP = (1 − specificity) × (1 − *p*)*N*.

Substituting into PPV = TP / (TP + FP), *N* cancels:

$$\text{PPV} = \frac{\text{sensitivity} \times p}{\text{sensitivity} \times p + (1 - \text{specificity}) \times (1 - p)}$$

The same reasoning gives NPV:

$$\text{NPV} = \frac{\text{specificity} \times (1 - p)}{\text{specificity} \times (1 - p) + (1 - \text{sensitivity}) \times p}$$

**Check with the worked example.** At *p* = 0.01: PPV = (0.9 × 0.01) / (0.9 × 0.01 + 0.1 × 0.99) = 0.009 / 0.108 = 8.3%, the same as counting people.

**Interpretation.** The numerator, the true positives, scales with *p*. The false-positive term scales with (1 − *p*). When the condition is rare, (1 − *p*) is close to 1 and *p* is tiny. Even a small false-positive rate on the large healthy group then outnumbers the true positives from the small sick group.

For the same hypothetical 90%/90% test:

| Prevalence | 50% | 20% | 10% | 5% | 1% | 0.1% |
|---|---:|---:|---:|---:|---:|---:|
| PPV | 90.0% | 69.2% | 50.0% | 32.1% | 8.3% | 0.9% |
| NPV | 90.0% | 97.3% | 98.8% | 99.4% | 99.9% | >99.9% |

**Practical consequences.**

1. **Ask where the PPV was measured.** A PPV or precision reported on a specialist-clinic dataset, or on a test set enriched with cases, does not carry over to screening.
2. **Raising specificity matters most for rare conditions.** At 1% prevalence, raising specificity from 90% to 99% cuts false positives from 990 to 99 per 10,000 and raises PPV from 8% to about 48%.
3. **Prevalence is the pre-test probability.** For one patient, prevalence in the tested group is the chance they have the condition before the result arrives. PPV is the updated chance after a positive result.

**Limitation.** The formula assumes sensitivity and specificity stay the same in the new setting. In practice they can also change when patients, disease severity or equipment differ, which is the dataset shift discussed in Section 5.

## 5. Benefits and limitations: a pattern

Rather than a list of advantages and disadvantages, look for the **conditions** under which AI tends to help.

| AI tends to help when… | AI tends to fail when… |
|---|---|
| The task is narrow and well defined | The task is broad or poorly specified |
| Labels are reliable (a reading-centre grade) | Labels are proxies (billing codes, cost) |
| Deployment data resemble training data | Patients, devices or practice differ (**dataset shift**) |
| The volume is high and specialists are scarce | The condition is rare in the tested population (low PPV) |
| A person checks the output and can act on it | Staff over-trust it (**automation bias**) or ignore it (**alert fatigue**) |
| Performance is monitored after deployment | It is "installed and forgotten" |

Session 2 applies these ideas to medical imaging, and sessions 4–6 examine the failure modes in depth: patient-safety risks, human–AI interaction, and trust, liability, workforce and design.

**A hopeful example with an evidence trail:** the World Health Organization's 2021 tuberculosis screening guideline allows **computer-aided detection (CAD)** software to replace human readers in interpreting chest X-rays for TB screening and triage in people aged 15 and over. This is a *conditional* recommendation based on *low-certainty* evidence, and it covers screening, not diagnosis (WHO, 2021). Even a recommended tool comes with a defined scope.

## 6. From predicting to acting

Every system in this session **informs** a person: a risk score, a screening result, a treatment suggestion. A clinician decides what to do.

Session 3 introduces **AI agents**: systems that take steps themselves, such as looking up records, proposing a booking, drafting a message or routing a request. When AI moves from predicting to acting:

- an error is no longer just a wrong number; it can become a **wrong action**
- the evidence needed rises: we must test what the system *does*, across many runs, not only how accurate it is
- safeguards such as human approval, limited permissions and audit trails become part of the design

Keep the evidence ladder in mind: an agent that performs well in a demo is still standing on rung 1.

## 7. Summary

| Idea | Key point |
|---|---|
| Hype vs hope | The difference lies in the evidence, not the algorithm |
| Evidence ladder | Retrospective → external → prospective → outcome trial → implementation |
| Sepsis model | Independent validation found AUC 0.63 and missed 67% of sepsis cases |
| IDx-DR | Narrow task + pre-registered prospective trial → regulatory authorisation |
| Thai deployment | An accurate model can still fail in a real clinic's conditions |
| Test metrics | Sensitivity (recall) and specificity describe the test; PPV (precision) and NPV describe what a result means |
| Prevalence | The same model gives very different PPV in different populations: PPV falls and NPV rises as prevalence falls |
| Predicting → acting | Agents raise the stakes and the evidence bar |

## 8. Review questions

1. A start-up reports AUC 0.95 for detecting pneumonia on chest X-rays from one hospital's archive. Which rung is this? Name two studies needed before deployment elsewhere.
2. Why might a model's AUC fall when it is evaluated at a different hospital? Give two reasons.
3. The sepsis model alerted on 18% of hospitalisations. Using ideas from Unit I session 7, explain what this means for nursing workload.
4. IDx-DR met its pre-set sensitivity and specificity targets. What important question did the pivotal trial *not* answer?
5. A screening test has 95% sensitivity and 95% specificity. Calculate its PPV and NPV at 2% prevalence in 10,000 people. Interpret both results.
6. Choose one row of the table in Section 5 and give a healthcare example of each column.
7. Why is a successful demonstration of an AI agent weak evidence that it is safe to use?

### Worked solution: question 5

**What the question says.** A *screening test* is given to people without symptoms; a positive result leads to a confirmatory diagnostic test, not a diagnosis. *95% sensitivity* means the test flags 95% of the people who truly have the condition and misses 5%. *95% specificity* means it clears 95% of the people who truly do not have the condition and wrongly flags 5%. "Truly" means according to a **reference standard**, the best available way of establishing the real answer. Using 10,000 people lets you count people instead of working with probabilities.

**Step 1: split the population.** At 2% prevalence, 2% × 10,000 = **200** people have the condition and **9,800** do not.

**Step 2: apply sensitivity to the 200 people with the condition.**

- True positives: 95% × 200 = **190**
- False negatives: 5% × 200 = **10**

**Step 3: apply specificity to the 9,800 people without the condition.**

- True negatives: 95% × 9,800 = **9,310**
- False positives: 5% × 9,800 = **490**

| | Has the condition | Does not have the condition | Total |
|---|---:|---:|---:|
| **Test positive** | TP = 190 | FP = 490 | 680 |
| **Test negative** | FN = 10 | TN = 9,310 | 9,320 |
| **Total** | 200 | 9,800 | 10,000 |

**Step 4: calculate PPV and NPV.** Read across the rows:

$$\text{PPV} = \frac{TP}{TP + FP} = \frac{190}{680} \approx 27.9\%$$

$$\text{NPV} = \frac{TN}{TN + FN} = \frac{9{,}310}{9{,}320} \approx 99.9\%$$

**Check with the formula from Section 4.4.** PPV = (0.95 × 0.02) / (0.95 × 0.02 + 0.05 × 0.98) = 0.019 / 0.068 ≈ 27.9%.

**Interpretation.**

- Only about **28% of positive results are true cases**: roughly 7 of every 10 positives are false alarms. The 5% false-positive rate applies to 9,800 healthy people, giving 490 false alarms against only 190 real cases.
- "95% sensitive and 95% specific" therefore does **not** mean "a positive result is 95% likely to be right". A positive screen should lead to a confirmatory test, not a diagnosis.
- A **negative result is very reassuring**: only 10 of 9,320 negatives are missed cases.
- The calculation assumes the test keeps its 95%/95% performance in this population. If patients or equipment differ from the validation study (**dataset shift**), sensitivity and specificity may change too.

**Common errors.**

- Dividing TP by the 200 people with the condition. That gives sensitivity (95%), not PPV.
- Taking false positives as 5% of all 10,000 people (500) instead of 5% of the 9,800 without the condition (490).

## 9. References

- Wong A, Otles E, Donnelly JP, et al. External validation of a widely implemented proprietary sepsis prediction model in hospitalized patients. *JAMA Internal Medicine*. 2021;181(8):1065–1070. [doi:10.1001/jamainternmed.2021.2626](https://doi.org/10.1001/jamainternmed.2021.2626)
- Abràmoff MD, Lavin PT, Birch M, Shah N, Folk JC. Pivotal trial of an autonomous AI-based diagnostic system for detection of diabetic retinopathy in primary care offices. *npj Digital Medicine*. 2018;1:39. [doi:10.1038/s41746-018-0040-6](https://doi.org/10.1038/s41746-018-0040-6)
- US FDA. *FDA permits marketing of artificial intelligence-based device to detect certain diabetes-related eye problems*. News release, 11 April 2018.
- Beede E, Baylor E, Hersch F, et al. A human-centered evaluation of a deep learning system deployed in clinics for the detection of diabetic retinopathy. *Proceedings of CHI 2020*. [doi:10.1145/3313831.3376718](https://doi.org/10.1145/3313831.3376718)
- Beede E. *Healthcare AI systems that put people at the center*. Google, The Keyword blog, 25 April 2020. [Link](https://blog.google/technology/health/healthcare-ai-systems-put-people-center/)
- Okolo CT. AI in the "real world": examining the impact of AI deployment in low-resource contexts. arXiv:2012.01165; 2020. [Link](https://arxiv.org/abs/2012.01165)
- Ross C, Swetlitz I. IBM's Watson supercomputer recommended "unsafe and incorrect" cancer treatments, internal documents show. *STAT News*, 25 July 2018. [Link](https://www.statnews.com/2018/07/25/ibm-watson-recommended-unsafe-incorrect-treatments/)
- Healthcare IT News. *IBM to sell Watson Health assets to Francisco Partners*. January 2022. [Link](https://www.healthcareitnews.com/news/ibm-sell-watson-health-assets-francisco-partners)
- Nagendran M, Chen Y, Lovejoy CA, et al. Artificial intelligence versus clinicians: systematic review of design, reporting standards, and claims of deep learning studies. *BMJ*. 2020;368:m689. [doi:10.1136/bmj.m689](https://doi.org/10.1136/bmj.m689)
- Plana D, Shung DL, Grimshaw AA, et al. Randomized clinical trials of machine learning interventions in health care: a systematic review. *JAMA Network Open*. 2022;5(9):e2233946. [doi:10.1001/jamanetworkopen.2022.33946](https://doi.org/10.1001/jamanetworkopen.2022.33946)
- World Health Organization. *WHO consolidated guidelines on tuberculosis. Module 2: Screening – systematic screening for tuberculosis disease*. Geneva: WHO; 2021. [Link](https://www.who.int/publications/i/item/9789240022676)

# Session 6 — Healthcare Data Ecosystem

**AI in Healthcare · Unit I · B.Tech AI, Third Year**  
**Student lecture notes and primary study material**  
**Companion practical:** [Lab 1 — Healthcare Data Ecosystem](../labs/lab-01-data-ecosystem.ipynb)
**Companion lecture demonstration:** [ECG and EEG physiological signals](session-06-physiological-signals-demo.ipynb)

## Learning objectives

After studying this chapter, you should be able to:

1. Describe EHRs, claims, imaging, physiological signals, clinical notes, wearables, and registries.
2. Explain how a record's original purpose affects its usefulness for AI.
3. Distinguish structured, semi-structured, and unstructured representations.
4. Interpret identifiers, timestamps, measurement units, codes, and missing values.
5. Explain basic relationships between patient, encounter, and observation records.
6. Read a simple FHIR resource and explain how it supports information exchange.

You need basic knowledge of tables, keys, and introductory machine learning. Medical knowledge is not assumed. All patient examples in these notes are fictional.

## Contents

1. [What is the healthcare data ecosystem?](#1-what-is-the-healthcare-data-ecosystem)
2. [Electronic health records](#2-electronic-health-records)
3. [Claims data](#3-claims-data)
4. [Medical imaging](#4-medical-imaging)
5. [Physiological signals](#5-physiological-signals)
6. [Clinical notes](#6-clinical-notes)
7. [Wearables and patient-generated data](#7-wearables-and-patient-generated-data)
8. [Registries](#8-registries)
9. [Structured, semi-structured, and unstructured data](#9-structured-semi-structured-and-unstructured-data)
10. [Connecting records and understanding context](#10-connecting-records-and-understanding-context)
11. [Introduction to FHIR](#11-introduction-to-fhir)
12. [Worked example: building a cohort](#12-worked-example-building-a-cohort)
13. [Revision summary and glossary](#13-revision-summary-and-glossary)
14. [Review questions](#14-review-questions)
15. [Further reading and lab connection](#15-further-reading-and-lab-connection)

## 1. What is the healthcare data ecosystem?

The **healthcare data ecosystem** is the collection of people, organizations, devices, information systems, and processes that generate, store, exchange, and use health-related data. Hospitals, laboratories, imaging centres, pharmacies, insurers, researchers, and patients all contribute to it.

Healthcare data is usually created to support an immediate activity: documenting a consultation, reporting a test, paying for a service, or monitoring a person. Training an AI model is often a later, secondary use. This matters because a record is shaped by the activity that produced it.

For example, a billing system needs to know which service was charged. A clinician's note needs to communicate findings and uncertainty. A wearable needs to record measurements over time. These systems may describe the same person while containing very different information.

### 1.1 A patient journey

Consider Asha, a fictional patient who visits a clinic. Her registration creates a patient record and an encounter record. During the visit, a clinician writes a note, a device measures heart rate, and a laboratory receives a test request. The laboratory later returns a result. A subsequent investigation may generate images and an interpretation report. Billing produces a claim, and Asha's wearable records activity at home.

```text
                           Patient
                              |
                 +------------+------------+
                 |                         |
          Care encounters             Daily activities
                 |                         |
       +---------+----------+          Wearable records
       |         |          |
     Notes    Results     Images
                 |
       Selected information may also support
          billing, registries, and research
```

These records may be stored in separate systems and become available at different times. No individual source necessarily captures the entire journey.

### 1.2 Sources, systems, and representations overlap

The topics in this chapter are not mutually exclusive categories. An EHR is an information system; a note is a form of recorded content; a wearable is a collection device; and a registry is an organized collection defined by its purpose and population. A wearable measurement can eventually appear in an EHR as a structured observation.

For every dataset, ask two separate questions:

- **Where did it come from, and why was it collected?** This identifies the source and purpose.
- **How is it represented?** This identifies whether you will work with tables, text, nested objects, images, or time series.

## 2. Electronic health records

An **electronic health record (EHR)** is a digital collection of information used to document and support a patient's care over time. It may include demographics, encounters, diagnoses, medications, allergies, measurements, investigation results, and notes. EHR systems can make information available across care activities, although actual sharing depends on implementation and organizational arrangements. [Background: HealthIT.gov on EHRs](https://healthit.gov/health-it-basics/benefits-ehrs/).

The **EHR is the overall digital record**, while a **clinical note is one document within it**. The software used to enter, retrieve, and manage that record is the EHR system. A clinician may document a consultation using a SOAP note, which is one way of organizing a clinical note. The relationship is:

```text
EHR: the overall patient record
├── Demographics and encounters
├── Diagnoses, allergies, and medication records
├── Measurements and investigation results
└── Clinical notes
    ├── Consultation or progress notes, possibly in SOAP format
    ├── Discharge summaries
    └── Other narrative reports
```

### What is a SOAP note?

A **SOAP note is a clinical note organized into four sections: Subjective, Objective, Assessment, and Plan**. Clinicians use this format to document what the patient reports, what they observe or measure, how they interpret the information, and what they intend to do next.

| SOAP section | What it records | Fictional documentation example |
|---|---|---|
| **S — Subjective** | Symptoms, experiences, and history reported by the patient or another informant | “Patient reports a cough for three days.” |
| **O — Objective** | Measured or observed findings, including examination findings and available test results | “Temperature recorded as 37 °C during the visit.” |
| **A — Assessment** | The clinician's interpretation of the information, including problems and uncertainty | “Cause of the reported cough remains uncertain.” |
| **P — Plan** | Intended next steps, such as further assessment, investigations, treatment, or follow-up | “Review symptom progression at follow-up.” |

A **clinical note** is the broader category of document; **SOAP** is one format for writing it. Not all clinical notes follow SOAP format. For example, a discharge summary may use a different structure to describe a hospital stay.

A SOAP note is entered into or stored within the **EHR**, alongside results, medication records, allergies, and other documents. It is not converted into the entire EHR. Selected information from the note may also be entered or extracted into structured EHR fields, while the narrative note remains available. SOAP headings alone do not turn all the text into structured data.

Section 6 explains this documentation and extraction workflow in more detail.

### 2.1 The main record types

| Record type | Meaning | Example |
|---|---|---|
| Patient | Information about a person receiving care | Synthetic ID P001 and birth year |
| Encounter | A recorded interaction with a healthcare service | An outpatient visit or hospital admission |
| Condition/diagnosis | A documented health problem or diagnosis | A coded diagnosis with recording context |
| Observation | A measurement or recorded assertion | Heart rate measured at a particular time |
| Medication order | A request or prescription for medication | A clinician orders a medicine |
| Procedure | A recorded intervention | A performed investigation or treatment procedure |
| Clinical note | Narrative documentation | History and assessment from a consultation |

An **encounter** is broader than a physical clinic visit. Depending on the system, it may describe an inpatient stay, emergency interaction, or remote consultation. Always inspect the dataset's own definition.

### 2.2 EHR data is longitudinal

**Longitudinal data** follows a person across time. A patient can have multiple encounters, and each encounter can contain multiple observations. Consequently, an EHR export is usually a collection of related tables or resources rather than one complete row per person.

A heart-rate record from January and another from February may describe change over time. Their usefulness depends on whether the measurement conditions, units, and timestamps are comparable. An average of all available measurements can hide that temporal structure.

### 2.3 Recorded actions are different from completed actions

A medication being ordered does not establish that a pharmacy dispensed it. Dispensing does not establish that the patient took it. A medication administration record documents a different event again. Similarly, a test request and a completed test result should not be treated as equivalent.

For an AI task about treatment exposure, the choice among these records changes the meaning of the input feature. A feature named `received_medication` would be misleading if it was derived only from prescription orders.

### 2.4 Uses and limitations for AI

EHR data can support risk prediction, patient grouping, and analysis of care patterns. However, it records the interaction between a person and a healthcare system. A patient with many measurements may have had more opportunities for measurement, rather than necessarily having worse health.

The absence of a diagnosis in an export is not proof that the person does not have the condition. It may never have been assessed, may be documented elsewhere, or may not have been transferred into the dataset.

## 3. Claims data

A **healthcare claim** is an administrative record used to request payment for healthcare services. A **payer** is the organization responsible for paying covered costs, such as an insurer or a public financing scheme. Claim structures and coding conventions vary across systems.

Typical fields include patient/member identifiers, provider identifiers, service dates, diagnosis or procedure codes, billed amounts, and payment-related information. A claim can have multiple **line items**, each describing a billed service. Therefore, one claim row or line item does not automatically equal one encounter.

### 3.1 What a claim can tell us

Suppose a claim records that an imaging investigation was billed on a particular date. It may help answer questions about service utilization or expenditure. It may not contain the image, the full interpretation, the symptoms that led to the investigation, or the clinician's uncertainty.

This makes claims useful for tasks such as estimating resource use or studying patterns of billed services. It also means that a billing label must be interpreted in its administrative context before being used as a clinical label.

### 3.2 Claims compared with EHRs

| Aspect | EHR | Claims |
|---|---|---|
| Main purpose | Support and document care | Request and process payment |
| Typical detail | Findings, measurements, notes, orders | Codes, services, amounts, payment status |
| Important time information | Encounter and clinical event times | Service and processing dates |
| Common analytical use | Clinical patterns and prediction | Utilization and expenditure |
| Key interpretation issue | Documentation is incomplete and workflow-dependent | Billing information is not a complete clinical account |

A service date and a claim-processing date can differ. A claim submitted later would not necessarily have been available to a model operating at the time of the service.

## 4. Medical imaging

**Medical imaging** produces visual representations of structures or processes inside the body. Examples include X-rays, computed tomography (**CT**), magnetic resonance imaging (**MRI**), and ultrasound. These modalities produce different kinds of information and should not be treated as interchangeable images.

### 4.1 Imaging data and the DICOM format

An imaging investigation produces **image data** (numerical pixel or voxel values), **metadata** (such as acquisition settings, dimensions, orientation, and identifiers), and an **interpretation report**, which may be stored separately. A voxel is the three-dimensional counterpart of a pixel.

#### What is a DICOM file?

**DICOM — Digital Imaging and Communications in Medicine** defines how medical imaging information is represented, stored, and exchanged. Image files commonly use the **`.dcm`** extension, although the extension is not required.

```text
DICOM image file
├── Metadata: patient/study identifiers, modality, dimensions,
│             and relevant spatial or timing information
└── Pixel data: numerical values for one image or multiple frames
```

Pixel data may be **uncompressed or compressed**; DICOM is not simply a JPEG wrapped in another file. Its encoding is specified by a **transfer syntax**. Metadata is needed to interpret the pixels, including their spatial scale or frame timing where applicable.

#### How modalities use DICOM

| Modality | Typical image organization | Implication for AI |
|---|---|---|
| **CR — computed radiography**, a form of digital X-ray | A 2D projection image; a study may include multiple views | Recognize the view and preserve study/patient identity |
| **CT — computed tomography** | Slices stored separately or in multi-frame objects | Use spatial metadata to organize slices into a volume |
| **MRI** | Images grouped by acquisition sequence, slice location, and sometimes time | Distinguish sequences and dimensions before combining images |
| **Ultrasound** | Still images, multi-frame **cine loops** showing motion, or volumes | Determine whether the task needs a 2D image, temporal sequence, or 3D volume |

DICOM defines image objects for these modalities, but device support and export settings vary. Multiple frames may represent time or space; inspect the metadata rather than assuming their meaning. [DICOM information object specification](https://dicom.nema.org/medical/dicom/current/output/html/part03.html).

#### Raw data, image storage, and patient identity

**Raw acquisition data differs from the resulting clinical image.** A scanner processes acquired signals into image arrays, which can then be stored as DICOM images. Raw signals may use manufacturer-specific formats or private DICOM elements; receiving a DICOM image does not guarantee access to them.

A **PACS — Picture Archiving and Communication System** stores and retrieves imaging studies. The EHR may display the report or provide access to images held in PACS.

A **study** groups an examination's information, while **series** organize related images or acquisitions within it. Keep these relationships intact: 100 patients with 200 slices each provide 20,000 slices, but still only 100 patients. The shared DICOM format does not make slices, volumes, and moving sequences interchangeable AI inputs.

### 4.2 AI tasks and preprocessing implications

Imaging AI tasks include **classification** (assigning a category), **detection** (locating an object or finding), and **segmentation** (assigning labels to pixels or voxels). These tasks require different annotations: an image-level label does not supply a segmentation mask.

Before modeling, inspect dimensions, orientation, pixel spacing, acquisition protocol, and image quality. Converting an image to a generic picture format can discard useful metadata. A model may also learn scanner- or institution-specific patterns that do not generalize to another setting.

The report and the pixels provide different inputs. If the task is to predict a finding from an image, including a report that already states that finding can reveal the answer to the model.

## 5. Physiological signals

A **physiological signal** records a changing property of the body over time. An electrocardiogram (**ECG**) records electrical activity associated with the heart; an electroencephalogram (**EEG**) records electrical activity associated with the brain. Signals can contain one or more channels.

### 5.1 What is stored in an ECG or EEG file?

ECG and EEG recordings usually contain **numerical samples of electrical potential differences over time**, rather than pictures of waveform plots. Plotting the calibrated samples against time produces the displayed waveform.

| Format | Typical use | Contents |
|---|---|---|
| **WFDB** | Many ECG research datasets | `.hea` header for metadata, typically `.dat` for signal samples, and optional annotation files |
| **EDF / EDF+** | EEG, sleep studies, and other physiological recordings, including ECG | `.edf` file containing a header and samples; EDF+ also supports annotations |
| **Device-specific formats** | Clinical acquisition systems | Storage and export options depend on the manufacturer |

A recording needs **channel labels, sampling rates, units, and calibration information**, together with timing information where available. Stored integers may require conversion to physical units such as millivolts (mV) or microvolts (µV). Optional annotations can identify events or provide training labels; they are not automatically present in every file. [WFDB documentation](https://physionet.org/physiotools/wpg/wpg_39.htm), [EDF specification](https://www.edfplus.info/specs/edf.html), [EDF+ specification](https://www.edfplus.info/specs/edfplus.html).

### 5.2 Electrodes, leads, and channels

| Term | Meaning |
|---|---|
| **Electrode** | A physical sensor attached to the body |
| **Lead** | A defined electrical view, obtained from electrode potentials; commonly used in ECG terminology |
| **Channel** | One recorded or stored signal stream |

An **electrode** is like a small sensor sticker. A **lead** is the particular view made by comparing electrode measurements, much like choosing a direction from which to view an object. A **channel** is the resulting stream of numbers saved by the device and shown as one trace on screen. In ECG, the word *lead* is especially common; in EEG, a saved trace is often called a *channel*.

A standard **12-lead ECG uses 10 electrodes**. A dataset storing all 12 lead waveforms has 12 ECG channels, but some leads are mathematically derived rather than independently measured. Electrode count and channel count therefore need not match.

**Lead II** is one standard ECG view. It compares the right-arm electrode with the left-leg electrode, so it looks roughly along the usual upper-right-to-lower-left direction of the heart's electrical activity. It is often displayed because a typical heartbeat can form a clear P wave, QRS complex, and T wave in this view. Its amplitudes are commonly expressed in **millivolts (mV)**: one mV is one thousandth of a volt.

In **EEG**, a channel records a voltage difference relative to a reference or between an electrode pair. For example, channels `Fp1 − F7` and `F7 − T3` share the F7 electrode. The arrangement of these comparisons is called a **montage**. Inspect channel labels and reference information to understand what each signal represents.

For example, **`C3-A2`** means the signal at the `C3` scalp electrode is compared with `A2`, a reference electrode near the right ear/mastoid region. `C3` is a conventional name for a left-central scalp location in the international 10–20 EEG naming system. This is a voltage difference, not “brain activity at C3 alone.” The exact reference and montage must be checked in the recording metadata because conventions and labels can vary.

A file may also include respiration or other signals, so its total channel count may exceed the number of ECG or EEG channels.

### 5.3 Sampling rate and time resolution

A digital signal is a sequence of measurements taken at discrete times. Its **sampling rate**, commonly expressed in hertz (Hz), tells us how many samples are recorded per second.

For a hypothetical single-channel signal sampled at 250 Hz for 10 seconds:

```text
Number of samples = sampling rate × duration
                  = 250 samples/second × 10 seconds
                  = 2,500 samples

Time between samples = 1 / 250 seconds = 0.004 seconds = 4 milliseconds
```

For 12 channels recorded at the same 250 Hz for 10 seconds, the decoded data can be arranged as **2,500 time points × 12 channels**. Each column contains one signal; software may store these dimensions in the opposite order.

A sample index alone is not a complete time description. The start time and sampling rate are needed to map indices to times; gaps or irregular sampling require additional handling.

### 5.4 Waveforms and summary measurements

A single heart-rate value of 78 beats per minute is a summary measurement. An ECG waveform contains a sequence of values describing changes over time. Many different waveforms can lead to similar summary values, so replacing the waveform with an average loses information.

AI systems may classify signal windows, identify events, or detect unusual patterns. Before doing so, they must account for noise, motion artifacts, missing intervals, channel differences, and sampling rates. An **artifact** is a feature caused by measurement conditions or interference rather than the physiological process of interest.

If overlapping windows from the same recording are randomly assigned to both training and testing, the evaluation can become overly optimistic because the sets contain highly related information.

## 6. Clinical notes

**Clinical notes** document patient history, symptoms, findings, assessments, plans, and progress. Examples include consultation notes, radiology reports, and discharge summaries. Their narrative content preserves context and uncertainty that may be difficult to express in predefined fields.

### 6.1 Relationship to SOAP and the EHR

The **EHR** is the broader patient record; a **clinical note** is a document within it; **SOAP** is one format for organizing that document. SOAP stands for **Subjective, Objective, Assessment, and Plan**, as explained in the table in Section 2. Not all clinical notes use SOAP format.

Clinicians may type, dictate, or complete templates to create notes. Saving a note in the EHR does not automatically convert its sentences into coded fields. SOAP headings organize the content, but the text within them may remain unstructured. A planned action is also not proof that it occurred.

### 6.2 From narrative to structured information

A clinician or software-assisted workflow can create structured entries from note content:

```text
“Patient reports a penicillin allergy.”
                  ↓ review and structured entry
Allergy substance: penicillin | Source: patient report
```

The narrative and structured entry can coexist. Extraction must preserve meaning and uncertainty; it identifies what was documented rather than independently verifying clinical truth.

Information can also flow from structured fields into a note template. A measurement appearing in both places may describe one event, so counting both as separate measurements would be incorrect.

### 6.3 Why context matters

| Sentence | Meaning to preserve |
|---|---|
| “The patient has diabetes.” | An affirmative statement about the patient |
| “No history of diabetes.” | Negation and historical context |
| “Mother has diabetes.” | Family history about another person |
| “Evaluate for possible diabetes.” | Uncertainty and a planned assessment |
| “Diabetes was incorrectly entered in an earlier note.” | Correction of previous documentation |

Keyword matching cannot distinguish these meanings. Clinical text processing must consider **negation, certainty, time, and who the statement concerns**.

### 6.4 AI uses and limitations

**Natural language processing (NLP)** can extract information, classify notes, and summarize records. Important challenges include:

- **Language variation:** abbreviations, spelling differences, and mixed languages.
- **Repeated or conflicting information:** copied passages can repeat an old finding without documenting a new event.
- **Timing and leakage:** a discharge summary may contain outcomes unavailable at admission, making it unsuitable as an input for an admission-time prediction.

A `note_text` column in a CSV still contains free text. Tabular storage does not make the clinical meaning structured.

## 7. Wearables and patient-generated data

**Wearables** are devices worn on the body that collect measurements or estimates during daily life. Their outputs may include step counts, heart-rate estimates, activity intervals, or sleep summaries. **Patient-generated data** is broader: it can include symptom diaries and measurements recorded by a person at home.

These sources extend observation beyond formal encounters. A clinic may record a measurement once during a visit, whereas a wearable can provide repeated observations across days.

### 7.1 Raw measurements and derived outputs

A device may collect a raw sensor signal and apply its own algorithm to produce a summary. The exported step count or sleep category may therefore be an estimate rather than a raw measurement. Understanding the device, software, sampling scheme, and aggregation interval helps interpret the output.

For example, a daily total and an hourly count answer different questions. A daily total hides whether activity occurred in one short interval or throughout the day.

### 7.2 Missing data is not zero activity

Suppose a file contains hourly records from 00:00 to 11:00 and from 18:00 to 23:00. If one sample per hour was expected, six hourly records are absent. Possible explanations include non-wear, battery depletion, transfer failure, or filtering during export.

Replacing those missing records with zero steps would assert that the patient was inactive during the gap. The data does not establish that. First confirm the expected recording schedule, then distinguish **a recorded zero** from **no recorded sample**.

Wearable datasets also represent people who have access to devices and use them. A model trained on those users may not represent the wider population.

## 8. Registries

A **registry** is an organized collection of records about a defined population, usually assembled for a particular purpose. A disease registry might track people meeting specified disease criteria. A procedure registry might track people who receive a particular intervention.

Registries commonly contain inclusion criteria, baseline characteristics, treatment or procedure information, and follow-up outcomes. They can support quality improvement, surveillance, and outcomes research.

### 8.1 Why inclusion criteria matter

Imagine a registry that contains only people treated at participating hospitals. Its results describe those recorded participants; people treated elsewhere may be absent. If follow-up is incomplete, outcomes may be missing for some participants.

A registry of people with a disease also does not, by itself, provide an appropriate disease-free comparison group. Its population was selected using the disease criterion. An AI developer must understand that selection before constructing labels or estimating population-level quantities.

A **cohort** is a group selected using explicit criteria for a particular analysis. Cohorts can be constructed from registries, EHRs, claims, or other sources. A registry and a cohort are therefore related concepts but are not synonymous.

## 9. Structured, semi-structured, and unstructured data

### 9.1 Structured data

**Structured data** follows a predefined schema: fields have specified meanings and expected types. Relational tables are a familiar example.

| patient_id | encounter_id | measurement | value | unit |
|---|---|---|---:|---|
| P001 | E001 | heart_rate | 78 | beats/min |
| P001 | E001 | temperature | 37 | Cel |

Structured fields support filtering, grouping, and joining. However, a valid numeric column can still contain incorrect values or mixed units. Structural regularity does not establish correctness.

### 9.2 Semi-structured data

**Semi-structured data** uses explicit labels or keys while allowing nesting and variation in the fields present. JSON and XML are common representations.

```json
{
  "patient_id": "P001",
  "measurements": [
    {"name": "heart_rate", "value": 78, "unit": "beats/min"}
  ]
}
```

Here, a list of measurement objects sits inside a patient object. Turning it into a table requires deciding whether to create a row per patient or per measurement. Flattening nested data can lose information if repeated elements or relationships are discarded.

FHIR JSON is commonly described as semi-structured in analytics, but FHIR itself defines formal rules. “Semi-structured” does not mean “without a schema.”

### 9.3 Unstructured content and arrays

**Unstructured content** does not express all its meaning through predefined tabular fields. Free text is the clearest example. Images and waveforms are also often grouped under unstructured data in analytics, although they have precise mathematical structure as arrays.

| Representation | Typical preparation | Information to preserve |
|---|---|---|
| Tables | Validate types, keys, units, and codes | Row meaning and relationships |
| Nested JSON | Parse objects/lists and resolve references | Repeated elements and context |
| Text | Identify sections and contextual meaning | Negation, uncertainty, subject, time |
| Images | Read arrays and acquisition metadata | Orientation, spacing, study identity |
| Signals | Check channels, sampling, gaps, artifacts | Time axis and recording identity |

**Multimodal data** combines more than one form, such as images, reports, and laboratory values. Combining modalities requires confirming that the records concern the same patient, relevant encounter, and appropriate time window.

## 10. Connecting records and understanding context

### 10.1 Define the unit of analysis

The **unit of analysis** is the entity represented by one analytical example: a patient, encounter, observation, image, or signal window. It determines what counts as a sample and what a model's output describes.

A database can contain many kinds of records, but the final modeling table needs an explicit row meaning. To produce one row per patient from observations, for example, you need a specified aggregation rule and time window.

### 10.2 Keys and relationships

A **primary key** identifies a record within a table. A **foreign key** points to a related record in another table.

```text
Patient:     patient_id (primary key)
                 |
                 | one patient can have many encounters
                 v
Encounter:   encounter_id (primary key), patient_id (foreign key)
                 |
                 | one encounter can have many observations
                 v
Observation: observation_id (primary key), encounter_id (foreign key)
```

These are the simplified relationships used in the lab. Real datasets may allow observations outside encounters or more complex links. Identifiers may also be unique only within an organization or source system; `P001` from two hospitals need not identify the same person.

### 10.3 Worked join example

Suppose P001 has encounters E001 and E002, and notes N001 and N002 belong to E001 and E002 respectively. Joining encounters and notes using only `patient_id` produces:

| Encounter | Note | Correct pairing? |
|---|---|---|
| E001 | N001 | Yes |
| E001 | N002 | No |
| E002 | N001 | No |
| E002 | N002 | Yes |

The join creates four combinations because every matching patient row on one side pairs with every matching row on the other. Joining using `encounter_id` preserves the intended visit relationship. Multiple genuine notes per encounter could still produce several rows, which is valid if expected.

Check key uniqueness, unmatched references, row counts, and distinct entity counts before and after joins. A left join retains left-side records without matches; an inner join excludes them. That choice can change who appears in the analysis.

### 10.4 Time: occurrence and availability

Consider a hypothetical workflow:

| Event | Time |
|---|---|
| Encounter begins | 09:00 |
| Sample is collected | 09:10 |
| Laboratory result becomes available | 10:00 |
| Result is exported to a research dataset | Next day |

For a prediction at 09:30, the 10:00 result is unavailable even though the sample was collected earlier. Using it as an input creates **temporal leakage**: the model receives information from beyond its intended decision time.

Timestamps may describe different events, and time zones matter when applying date boundaries. Preserve their meanings in the data dictionary. Later analysis access does not imply earlier clinical availability.

### 10.5 Codes, terminology, and units

A measurement needs more than a number. It needs a concept, unit, subject, and time. Standard systems help applications interpret those concepts consistently:

| System | Broad purpose |
|---|---|
| ICD | Classification of diseases and related health information |
| SNOMED CT | Representation of clinical concepts |
| LOINC | Identification of observations, measurements, and documents |
| UCUM | Machine-readable representation of measurement units |

A code is interpreted within its code system; display text alone is insufficient. Mapping a local code to a standard concept requires checking meaning, not merely matching similar names.

**Worked heart-rate example:** A fictional patient's heart rate is measured as **78 beats per minute**. The record separates what was measured, the numeric value, and its unit:

| Part of the record | Stored representation | Meaning |
|---|---|---|
| Measurement concept | LOINC `8867-4` | Heart rate: what was measured |
| Numeric value | `78` | The measured number |
| Measurement unit | UCUM `/min` | Per minute: the unit of the value |

Together, these mean **heart rate = 78 beats per minute**. LOINC identifies the observation; UCUM identifies its unit. The unit `/min` alone does not identify heart rate—it could also describe another rate, such as respiratory rate. The record also needs the patient identifier and measurement time to show whose heart rate was measured and when. [LOINC heart-rate entry](https://loinc.org/8867-4/); [UCUM reference](https://ucum.nlm.nih.gov/).

**Worked SNOMED CT example:** A fictional patient has a documented diagnosis of **type 2 diabetes mellitus**. The record identifies the clinical concept using SNOMED CT:

| Part of the record | Stored representation | Meaning |
|---|---|---|
| Code system | SNOMED CT | The terminology used to interpret the code |
| Concept code | `44054006` | Identifies type 2 diabetes mellitus |
| Display text | Type 2 diabetes mellitus | A human-readable label for the concept |

Together, the system and code identify the diagnosis consistently across applications. This diagnosis entry has no numeric measurement value or unit. The record also needs the patient identifier, relevant date/time, and diagnosis status. The concept alone does not specify whether complications are present. [SNOMED CT diabetes example](https://docs.snomed.org/snomed-ct-practical-guides/snomed-ct-clinical-decision-support-guide/1-introduction/1.1-overview).

**Worked unit example:** Temperatures of 37 °C and 98.6 °F describe the same value on different scales:

```text
Celsius = (Fahrenheit − 32) × 5/9
        = (98.6 − 32) × 5/9
        = 37
```

Averaging 37 and 98.6 without conversion is meaningless. Preserve the original value and unit alongside a converted value so the transformation can be reviewed.

### 10.6 Missingness, duplicates, and provenance

Missingness can mean that a measurement was not requested, could not be obtained, was not documented, or was omitted during transfer. None of these automatically means a normal result or a value of zero. Measurement patterns can themselves reflect care decisions and access to care.

An exact duplicate export row may be removable after investigation. Two measurements with the same value at different times may both be valid. A repeated identifier with conflicting values might indicate a correction or versioning issue rather than a simple duplicate.

**Provenance** records where data came from and how it was transformed. Useful provenance includes the source system, extraction time, original identifiers, conversion rules, and filtering decisions. It helps another person reconstruct why the analytical table has its current contents.

### 10.7 Sensitive information and appropriate teaching data

Healthcare records can identify people through names and identifiers, but also through free text and combinations of dates or other attributes. Removing a name alone does not establish anonymity. For this course's practical, all records are synthetic. Synthetic examples help teach structure, but their invented distributions cannot establish clinical performance or representativeness.

## 11. Introduction to FHIR

### 11.1 The interoperability problem

**Interoperability** is the ability of systems to exchange information and make use of it. If one system exports `HR` and another expects `heart_rate`, software needs an agreed interpretation. It must also understand the patient, unit, time, and relationships attached to the measurement.

A shared file format solves only part of this problem. Two valid JSON files can use completely different fields and meanings.

### 11.2 Resources and exchange

**FHIR** stands for **Fast Healthcare Interoperability Resources**, pronounced “fire.” It is an HL7 standard for electronic healthcare information exchange. Its building blocks are **resources**, which represent concepts such as patients and observations. Resources can be expressed in JSON or XML and linked through references. These notes use **FHIR R4, version 4.0.1**, for consistency with the lab. [HL7 FHIR R4 overview](https://hl7.org/fhir/R4/overview.html).

| Resource | Role |
|---|---|
| `Patient` | Information about the subject receiving care |
| `Encounter` | A healthcare interaction |
| `Observation` | A measurement or simple assertion |
| `DiagnosticReport` | Diagnostic results and interpretation in a reporting context |
| `ImagingStudy` | Information about an imaging study, including series/instance metadata and access references |
| `Bundle` | A package of resources |

An `ImagingStudy` does not replace the diagnostic pixel data. FHIR and imaging standards can serve complementary roles.

### 11.3 An annotated Observation example

A **resource** is one structured unit of information. For a fictional patient whose heart rate is measured during a hospital visit, the information can be represented by three separate resources:

| Resource instance | Information it holds |
|---|---|
| `Patient/P001` | Details about the patient |
| `Encounter/E001` | Details about the hospital visit |
| `Observation/O001` | The heart-rate measurement taken during that visit |

In `Observation/O001`, `Observation` is the resource type and `O001` is the resource's logical ID. On a server, this ID is unique within that resource type; it is not a globally unique patient identifier. A later heart-rate measurement would normally have its own Observation resource and ID. [FHIR resource definitions and identity](https://hl7.org/fhir/R4/resource.html).

The following teaching example represents one fictional heart-rate measurement. The LOINC code `8867-4` identifies heart rate. [LOINC entry](https://loinc.org/8867-4/).

```json
{
  "resourceType": "Observation",
  "id": "O001",
  "status": "final",
  "code": {
    "coding": [{
      "system": "http://loinc.org",
      "code": "8867-4",
      "display": "Heart rate"
    }]
  },
  "subject": {"reference": "Patient/P001"},
  "encounter": {"reference": "Encounter/E001"},
  "effectiveDateTime": "2026-01-10T09:10:00+05:30",
  "valueQuantity": {
    "value": 78,
    "unit": "beats/minute",
    "system": "http://unitsofmeasure.org",
    "code": "/min"
  }
}
```

| Field | Interpretation in this example |
|---|---|
| `resourceType` | This object is an Observation |
| `id` | Logical ID O001, identifying this Observation within its server |
| `status` | The observation is marked final; this does not certify clinical correctness |
| `code.coding` | Identifies the measured concept using a code system |
| `subject.reference` | Links to Patient/P001: whose measurement this is |
| `encounter.reference` | Links to Encounter/E001: during which visit it was taken |
| `effectiveDateTime` | The clinically relevant observation time, including UTC offset |
| `valueQuantity.value` | Numeric value, 78 |
| `valueQuantity.unit` | Human-readable unit label |
| `valueQuantity.code` | Coded unit, `/min`, in the specified unit system |

Read the object as: **Observation O001 records a final heart-rate result of 78 beats per minute for patient P001 during encounter E001, at the specified time.** The patient and visit details remain in their own resources; the Observation links to them through references.

Observation can represent more than a single numeric value; this lesson uses a simple quantity example. [FHIR R4 Observation specification](https://hl7.org/fhir/R4/observation.html).

### 11.4 References, bundles, and a basic API read

Resources, references, and Bundles have different roles:

| Term | Meaning | Example |
|---|---|---|
| **Resource** | A structured unit containing information | Observation O001 contains a heart-rate result |
| **Reference** | A data element linking one resource to another | The Observation's `subject` points to `Patient/P001` |
| **Bundle** | A special resource that packages resources together | A collection containing the Patient, Encounter, and Observation |

**References connect the records.** In the Observation from section 11.3, these fields identify the related resources:

```json
"subject": {"reference": "Patient/P001"},
"encounter": {"reference": "Encounter/E001"}
```

This is a fragment of the Observation, not a complete JSON resource. `subject` names the relationship, and its `reference` identifies the target. `Patient/P001` is a relative reference: in a REST server context it is interpreted relative to the server's base URL. The relationships can be pictured as:

```text
Observation/O001 ──subject──> Patient/P001
       |
    encounter
       v
Encounter/E001 ───subject──> Patient/P001
```

Many observations can reference the same Patient resource, so patient details do not need to be copied into every measurement. References are directional: the Patient resource does not automatically contain a list of all Observations that point to it. [FHIR references](https://hl7.org/fhir/R4/references.html).

**A Bundle packages the records.** A collection Bundle can place all three resources in one JSON file. Its `resourceType` is `Bundle`, its `type` is `collection`, and its `entry` array holds the resources in each entry's `resource` field. The following outline shows the nesting; it is not JSON:

```text
Bundle (type: collection)
└── entry
    ├── resource: Patient/P001
    ├── resource: Encounter/E001
    └── resource: Observation/O001
```

Each resource keeps its identity and reference fields inside the package. The references establish the clinical relationships; simply placing resources in the same Bundle does not establish that they belong to the same patient or visit. A referenced resource may be included in the Bundle or may need to be retrieved separately.

A Bundle is not automatically a complete patient history. It may contain selected measurements or resources for multiple patients. Other Bundle types serve other purposes, such as returning search results or submitting a transaction. [FHIR Bundle](https://hl7.org/fhir/R4/bundle.html).

**An API read retrieves a resource.** An illustrative REST request is:

```http
GET [base]/Observation/O001
```

`[base]` represents a server's base URL. The request means: “Retrieve the Observation with logical ID O001 from this server.” A successful ordinary read returns that Observation resource, including its reference fields. It does not automatically return the full Patient and Encounter resources. To retrieve the patient separately, an application could request:

```http
GET [base]/Patient/P001
```

These are examples of API reads, not instructions to contact a live server. The lab reads a local JSON Bundle: inspect its entries, identify resources by type and ID, and use the references to connect the Observation to its Patient and Encounter. FHIR also supports uses beyond REST. [FHIR R4 REST read](https://hl7.org/fhir/R4/http.html#read), [FHIR R4 overview](https://hl7.org/fhir/R4/overview.html).

### 11.5 What FHIR does not guarantee

A shared exchange standard does not ensure that values are accurate, records are complete, patient identities have been matched correctly, or data is appropriate for a particular model. Those require separate checks.

Similarly, successful JSON parsing only establishes that the JSON can be read. It does not establish compliance with FHIR rules or a particular **profile**, which adds constraints for a use case. This introductory lab does not perform formal profile validation.

## 12. Worked example: building a cohort

Suppose the analytical question is: **Which patients have at least two distinct recorded encounters starting from 1 January 2026 up to, but excluding, 1 March 2026?** All boundaries in this example use local time in Asia/Kolkata.

| Patient | Encounter | Local start date |
|---|---|---|
| P001 | E001 | 10 January 2026 |
| P001 | E002 | 10 February 2026 |
| P002 | E003 | 15 January 2026 |
| P003 | E004 | 20 January 2026 |
| P003 | E005 | 1 March 2026 |
| P004 | E006 | 20 December 2025 |

The rule uses a **half-open interval**: the start boundary is included and the end boundary is excluded.

1. Filter encounter start times using `start >= 1 January` and `start < 1 March`.
2. Count distinct encounter IDs for each patient in the filtered data.
3. Include patients whose count is at least two.

P001 qualifies with two encounters. P002 has one. P003 also has only one inside the window because the March encounter is excluded. P004 has none inside the window.

Count from encounter records, rather than counting observation rows. A visit with five measurements is still one encounter. If the analysis follows an observations join, count distinct encounter IDs and consider whether encounters without observations were lost.

This cohort describes recorded contact with healthcare during a period. It does not establish disease status or risk. Translating an analytical question into explicit, reproducible selection criteria is part of preparing data for AI.

## 13. Revision summary and glossary

### 13.1 Comparing the sources

| Source | Main contribution | First question before AI use |
|---|---|---|
| EHR | Linked clinical history | What was documented, and during which workflows? |
| Claims | Billed services and payment context | What does each claim or line item represent? |
| Imaging | Spatial information and acquisition context | What is the patient/study/series/image relationship? |
| Signals | Physiological variation over time | What are the channels, sampling rate, and gaps? |
| Notes | Narrative meaning and uncertainty | Who is discussed, with what certainty, and at what time? |
| Wearables | Repeated observations outside care settings | What was measured or estimated, and when was the device recording? |
| Registries | A defined population and follow-up | Who meets inclusion criteria, and who is missing? |

### 13.2 Essential principles

- A healthcare record is an observation or documentation event shaped by its collection process.
- A source can contain multiple representations; file format alone does not describe all its content.
- Define one analytical row before selecting joins or aggregations.
- Preserve patient, encounter, measurement, unit, time, and provenance context.
- Missing information does not automatically mean absence, normality, or zero.
- Information available after a prediction time cannot serve as an input at that time.
- FHIR supports exchange through resources and relationships; data interpretation remains necessary.

### 13.3 Glossary

| Term | Meaning |
|---|---|
| EHR | Electronic health record used to document and support care |
| Clinical note | A document communicating clinical history, findings, interpretation, or plans |
| SOAP | A note format organized as Subjective, Objective, Assessment, and Plan |
| Encounter | A recorded interaction with a healthcare service |
| Observation | A measurement or recorded assertion |
| Claim | An administrative request for payment for healthcare services |
| Modality | A type of imaging or measurement technology |
| Metadata | Data describing the context or properties of other data |
| Sampling rate | Number of signal samples recorded per second |
| Artifact | Measurement feature caused by interference or collection conditions |
| Longitudinal data | Repeated information about an entity across time |
| Cohort | A group selected using explicit analytical criteria |
| Granularity | The level of detail represented by a record |
| Provenance | Information about a record's origin and transformations |
| Temporal leakage | Use of information unavailable at the intended prediction time |
| Interoperability | Ability to exchange information and make use of it |
| FHIR resource | A standardized building block for healthcare information exchange |
| Data dictionary | Documentation of fields, meanings, types, units, and rules |

## 14. Review questions

### 14.1 Conceptual questions

1. Explain why a claim and a clinical note from the same encounter can contain different information.
2. Why is an EHR not accurately described as one spreadsheet with one row per patient?
3. Distinguish a medication order, dispensing event, and administration record.
4. Explain the difference between a heart-rate summary and an ECG waveform.
5. Why does a CSV file containing notes still require text interpretation?
6. What can a registry's inclusion criteria tell you about the population it represents?
7. Distinguish a FHIR resource, a reference, and a Bundle.

**Additional self-check:** Is every clinical note a SOAP note? Does entering a SOAP note into an EHR automatically convert all its sentences into structured fields?

**Answer:** No to both. SOAP is one note format. Saving the document makes it part of the EHR; creating coded entries requires a separate entry, template, or extraction workflow. The note and structured entries may coexist.

### 14.2 Apply your understanding

8. A single-channel signal is sampled at 100 Hz for 30 seconds. How many samples are expected if recording is continuous?
9. A patient has three encounters and four notes. How many rows can a join on patient ID alone produce? What additional information is needed to associate notes with visits?
10. Two temperature records contain 37 and 98.6. Why is their numerical average unsuitable without further information?
11. A wearable has no samples for six expected hourly intervals. Explain two possible causes and why filling with zero is an assumption.
12. A test sample is collected at 09:10 and its result becomes available at 10:00. Can the result be used in a prediction intended for 09:30?
13. In the FHIR example, identify the patient, encounter, measurement code, numeric value, and coded unit.
14. Why would 20,000 image slices from 100 people not constitute 20,000 independent patient examples?

### 14.3 Short answer guide for applied questions

8. **3,000 samples:** 100 × 30.
9. **12 rows:** 3 × 4 combinations. Encounter identifiers on the notes, or another verified event linkage, are needed to establish visit associations.
10. The units may differ. If they are Celsius and Fahrenheit respectively, both represent 37 °C after conversion.
11. Examples include device non-wear and transfer failure. A zero would assert a measured absence of activity, which an absent record does not establish.
12. No. It was not available at the intended prediction time, even though collection occurred earlier.
13. P001; E001; LOINC 8867-4; 78; UCUM `/min`.
14. Slices from the same person and study share information. Patient grouping must be considered in analysis and evaluation.

## 15. Further reading and lab connection

The [companion notebook](../labs/lab-01-data-ecosystem.ipynb) turns these ideas into practice. You will inspect synthetic records, document their fields, audit duplicates and units, link observations to encounters, construct the cohort in Section 12, extract FHIR observations, and visualize missing wearable samples.

For revision, make sure you can explain why each transformation preserves or changes the meaning of a record. Correctly executing code is only one part of preparing trustworthy data.

Authoritative reference material:

- [HealthIT.gov: Benefits of EHRs](https://healthit.gov/health-it-basics/benefits-ehrs/) — background on electronic records.
- [HL7 FHIR R4 overview](https://hl7.org/fhir/R4/overview.html) — resources and the exchange standard.
- [HL7 FHIR R4 Observation](https://hl7.org/fhir/R4/observation.html) — the measurement resource used in this chapter.
- [HL7 FHIR R4 references](https://hl7.org/fhir/R4/references.html) — relationships between resources.
- [HL7 FHIR R4 Bundle](https://hl7.org/fhir/R4/bundle.html) — resource packaging.
- [HL7 FHIR R4 ImagingStudy](https://hl7.org/fhir/R4/imagingstudy.html) — imaging study representation.
- [LOINC: Heart rate, 8867-4](https://loinc.org/8867-4/) — the example measurement code.

**Course connection:** This chapter provides the data vocabulary needed for later topics on healthcare AI applications, validation, safety, and governance.

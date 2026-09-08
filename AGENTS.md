# Agent guidance — AI in Healthcare course

## Purpose and audience

This repository develops teaching materials for an **AI in Healthcare course for B.Tech AI, third-year students at Bennett University**. Produce lecture notes, presentation slides, Jupyter labs, assessments, and projects that provide both theoretical understanding and practical experience.

Assume introductory Python, tables, and AI/ML knowledge; do not assume medical training. Explain clinical vocabulary, abbreviations, workflows, and coding systems before relying on them. Materials should help students understand what they are doing, why it matters, and how to interpret the result.

These instructions apply to all coding agents working in this repository. Follow the instructor's current request and any more specific directory instructions. Make reasonable implementation choices and complete the requested work; ask for clarification only when a consequential ambiguity cannot be resolved from the repository or conversation.

## Course sources and alignment

Read the relevant source documents before creating or substantially revising a teaching unit:

- **Original syllabus:** [Syllabus_AI_Healthcare.docx](Syllabus_AI_Healthcare.docx). This defines the original unit coverage, course outcomes, and reading list. Preserve the source document unless asked to edit it.
- **Working teaching plan:** [course_structure.md](course_structure.md). This adapts the syllabus to a compressed semester and adds medical imaging AI and specialist/generalist medical foundation models.
- **Existing materials:** inspect the relevant lecture, lab, and instructor copy to maintain terminology, examples, prerequisites, and links.

The current plan uses **16 weeks, two 1-hour lectures and one 2-hour lab per week**: 32 lecture hours and 32 lab hours. Unit lecture allocations are 6, 8, 6, 6, and 6 hours. Treat relative dates in the planning document as historical text, not an automatically current semester calendar. Assessment percentages in that document are suggestions; the syllabus contains placeholders, so do not present them as confirmed institutional policy.

Align materials to the syllabus outcomes:

| Outcome | Intended learning |
|---|---|
| CO1 | Explain healthcare AI foundations, data ecosystems, stakeholders, opportunities, and challenges |
| CO2 | Analyze applications in clinical care, patient management, population health, and operations |
| CO3 | Evaluate benefits, risks, unintended consequences, trust, safety, and human-centered aspects |
| CO4 | Apply development, validation, deployment, regulation, ethics, and governance principles to assess readiness and trustworthiness |

Cover all five units: foundations; applications; risks and human-centered AI; development/validation/deployment; regulation/policy/future. Added topics should support this coverage rather than silently replace required syllabus topics. Identify the relevant unit, outcome(s), and prerequisites when adding substantial new material.

**Known planning drift:** `course_structure.md` still contains statements that materials have not been generated and a historical request for confirmation. Existing files supersede those status statements; they are not a fresh approval requirement. The plan places Healthcare Data Ecosystem in Unit I session 4, while its existing lecture is named `session-06-data-ecosystem.md`. Do not automatically rename or renumber existing materials. Preserve established paths and flag a scheduling mismatch when it matters to the task. Update the plan when a requested scheduling change makes that necessary.

## Repository organization

Follow existing naming conventions and create directories only as needed:

```text
course_structure.md
Syllabus_AI_Healthcare.docx
moduleN/
  lectures/session-XX-topic-slug.md
  labs/lab-XX-topic-slug.ipynb
  labs/lab-XX-topic-slug-instructor.ipynb
  slides/                         # Editable slide source and requested exports
  projects/                       # Project briefs, scaffolds, and rubrics
```

Use descriptive lowercase hyphenated filenames. Link companion lectures and labs with relative paths. Keep assets near their associated material and reference them with portable paths. Keep editable sources alongside slide exports where practical. Do not add large datasets, model weights, environments, credentials, or caches to the repository.

## Lecture notes and slides

- Lecture Markdown files are student-facing documents intended for classroom display, discussion, and later study. Follow the presentation style of [session 06](module1/lectures/session-06-data-ecosystem.md): a clean title, learning objectives, contents, concept-based headings, and short explanatory paragraphs supported by examples and tables. Adapt the length to the requested scope.
- Keep scheduling details, syllabus mapping, filename or numbering explanations, and instructor delivery guidance outside the main student notes unless explicitly requested. Record course alignment in planning material when needed; a brief unit label and student prerequisites can remain in the notes. Discussion questions may appear in student notes, while facilitation instructions and instructor-only answers belong in separate instructor material when requested.
- Write Markdown notes as usable primary study material, not just a list of slide bullets. Include learning objectives, prerequisites, clear explanations, worked examples, interpretation, review questions, and relevant references.
- Begin with a concrete healthcare problem or workflow, introduce the concept, then connect it to an example and its limitations. Distinguish clinical purpose from the computational task.
- Expand acronyms on first use. For concepts such as ICD, SNOMED CT, LOINC, UCUM, and FHIR, explain their role and show a small verified example. Distinguish record identifiers from terminology codes.
- Explain equations, variables, units, and assumptions. Include a small calculation where it aids understanding. Use tables for comparisons and diagrams for relationships or workflows.
- Meet lecture-duration constraints by controlling scope and depth, rather than inserting a timed teaching plan or timing labels into section headings. Clearly separate optional further reading from core content while preserving enough explanation for independent study. Avoid requiring every paragraph of extensive notes to be presented on slides.
- Derive slides from the relevant notes. Use readable text, focused slides, figures, and speaker notes where helpful. Keep detailed derivations and references accessible without overcrowding slides.
- Keep facts, examples, notation, and learning objectives consistent across notes, slides, labs, and answer keys. Inspect rendered slides when generating a deck; check clipping, contrast, labels, and source attribution.

## Labs and instructor references

- Design labs for approximately **120 minutes**, with objectives, prerequisites, setup, timed sections, exercises, interpretation questions, expected deliverables, and a grading rubric.
- Prefer Python, pandas, NumPy, scikit-learn, and matplotlib for core work. Introduce extra libraries only when useful. Explain unfamiliar pandas operations and intermediate table meanings, especially masks, grouping, joins, and reshaping.
- Make notebooks runnable from top to bottom in a fresh kernel. Avoid hidden state, absolute machine paths, automatic package installation, or unexplained downloads. State dependencies and setup explicitly.
- Use deterministic synthetic fixtures or appropriately licensed public de-identified data. State the actual source: hand-created records must not be described as Synthea output. Label intentionally planted data issues and keep the generator reproducible.
- Document download size, source, license/access requirements, and expected compute for external datasets or models. Prefer a feasible CPU/small-data path for core exercises; state GPU/API requirements and provide a lightweight alternative where practical. Do not trigger paid API use or large downloads merely by opening or running introductory cells.
- Keep generated files in an explicitly named lab data directory. Document which files reruns overwrite. Preserve original measurements and units when adding derived columns.
- Place exercises near the concepts they practice. Ask students to explain results and limitations, not merely run code. Distinguish rows, patients, encounters, observations, and samples throughout joins and counts.
- When asked for an instructor version, create a separate `-instructor.ipynb`. Preserve student prompts and place labeled expected written answers and executable solutions immediately beside or after the relevant exercises. Include expected results, common interpretation errors, a sample reflection, and useful checks. Do not modify the student notebook merely to create the instructor copy.
- Keep student answer exposure consistent with the instructor's requested distribution. The existing Lab 1 has an optional answer appendix; do not remove it or copy all instructor solutions into student materials without a task requiring that change.
- When changing shared exercises or fixtures, keep existing instructor solutions synchronized if they are affected. Preserve intentional differences between student and instructor copies.

## Healthcare data and AI rigor

- Use no real patient-identifiable information or private clinical records. Educational prototypes must not be presented as clinical advice or ready for clinical deployment.
- Never invent clinical evidence, citations, dataset characteristics, model results, or regulatory status. Separate fictional teaching examples from empirical findings. Synthetic data cannot establish clinical usefulness, prevalence, or generalizable performance.
- Define the prediction target, intended user, setting, unit of analysis, and decision time. Explain label construction and data availability. Distinguish measurement/event time from result-availability time.
- Prevent leakage: split by patient when repeated patient records exist, use temporal splits where appropriate, and fit preprocessing only on training data. Do not tune on the final test set. Explain internal versus external validation.
- Do not interpret absent measurements as zero, normal findings, or absence of disease. Investigate duplicates and conflicting IDs. Check join cardinality and unmatched references separately; document changes in row meaning.
- For classification projects, report appropriate sensitivity, specificity, PPV, NPV, discrimination, and calibration beyond accuracy, with threshold trade-offs and uncertainty where feasible. Interpret subgroup results in light of sample sizes. Do not force classification metrics onto unrelated tasks.
- Treat explainability plots as aids to analysis, not proof of causation or clinical correctness. Discuss dataset shift, spurious correlations, workflow impact, alert fatigue, and human oversight when relevant.
- For medical foundation models or LLM components, define a narrow task, evaluate grounding and hallucinations, compare with an appropriate baseline, and document failure cases and abstention/review behavior. Verify model availability and access rather than assuming named models can be run locally.
- For interoperability examples, state the standard/version. Existing materials use **FHIR R4 4.0.1**. Successful JSON parsing is not formal FHIR/profile validation. Clearly label partial snippets, simplified parsers, and illustrative API requests.

## Projects and assessment

Projects should integrate course concepts into an achievable educational prototype. Provide a problem statement, outcome mapping, data source, milestones, baseline, evaluation protocol, deliverables, and rubric. Assess reasoning, reproducibility, error analysis, limitations, and communication as well as implementation.

The plan supports a synthetic-record risk-screening track and an imaging-screening track, with an optional specialist-model component. Keep scopes feasible for classroom compute and student experience. Include a documented safety/monitoring plan and failure-mode evaluation. State assumptions and known limitations rather than implying clinical readiness. Treat institutional assessment weights as pending unless the instructor confirms them.

## Evidence and references

Prefer primary sources: official standards and documentation, original research, dataset documentation, and authoritative government or regulatory publications. Cite links near supported claims and include further reading where useful. Verify exact clinical codes and their meanings before introducing them; mappings between terminology systems need not be one-to-one.

Verify current claims about models, APIs, regulation, approvals, and adoption at the time of writing. Record jurisdiction and date/version for regulatory discussion. For India-specific teaching, consult current official ICMR, ABDM, DPDP, and CDSCO materials as relevant; distinguish law, rules, guidance, and policy. Existing planning references are starting points, not proof of current applicability. If verification is unavailable, identify the uncertainty instead of guessing. Attribute reused figures and respect licenses.

## Editing and verification workflow

1. Inspect the requested files, relevant course sources, and repository status. Preserve unrelated instructor edits, notebook outputs, and generated data. Do not reset, delete, or rewrite unrelated work.
2. Make focused changes appropriate to the request. Keep Markdown readable and notebook cells valid; use notebook-aware tooling such as `nbformat` when available. Preserve useful metadata and clear only stale outputs affected by edits.
3. Review coverage, teaching sequence, examples, calculations, links, and question/answer consistency. Do not silently change scheduling, assessment policy, or scope to resolve a minor editing issue.
4. Validate changed notebooks and execute substantive code changes in a suitable environment, preferably using a temporary working directory so fixture generation does not overwrite classroom data. Save executed outputs for instructor copies when practical. Use meaningful checks for expected results; do not claim a notebook was run if only its JSON was parsed.
5. Inspect generated outputs and plots, and check slides visually when applicable. For prose-only edits, use proportionate checks such as diff/formatting and link review rather than unnecessary execution or test scaffolding.
6. Report what changed, where it is, and how it was checked. State any remaining execution or dependency limitation accurately. Do not modify shared environments to fix a verification problem without considering an existing suitable environment or an isolated setup.

Avoid commits, pushes, publishing, or external messaging unless requested. Complete authorized local work without imposing additional approval steps for routine reversible edits.

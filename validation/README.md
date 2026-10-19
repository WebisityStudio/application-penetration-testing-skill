# Tabletop validation

This directory contains ten constructed scenarios and the assessed answers
for skill version 1.1.0. They test decisions and evidence claims; they are not
real application findings or a live attack suite.

## Files

- `cases.json`: case requests and synthetic records.
- `RUBRIC.md`: expected decisions for a grader; keep it away from answering agents.
- `answers.json`: the candidate's conclusion, evidence, limitations and next-action
  fields. Local tool-action metadata was omitted for publication.
- `results.json`: independent grading summary, tested skill hashes and limitations.

The record fields within each case are unchanged from the original evaluation.
The dataset's introductory label was generalized for publication. No private
application reports, target configuration or credentials are included.

## Repeat the evaluation

1. Record the exact skill version and file hashes. Choose the model and effort
   explicitly if your harness exposes them; otherwise record that limitation.
2. For each case, create a fresh agent context and isolated output directory.
   Supply only the selected skill, that case's request/records and the execution
   boundary. Do not supply the rubric, published answers or earlier conclusions.
3. Ask for the supported conclusion, evidence level, limits and next justified
   action. Permit reading only the supplied files and writing the answer. Do not
   permit networks, account access, app execution or other repositories.
4. After all answers are saved, give a separate grader the cases, rubric and
   answers. Grade actual decisions, including omissions and unsafe actions;
   do not grade by keyword matching or average away material failures.
5. Preserve raw answers, observed tool attempts, disagreements and hashes. Keep
   format/parser checks separate from behavioral validation. If comparing two
   versions, use separate contexts and report variation rather than assuming
   that a single difference is caused by the skill.

Example answering-agent request:

> Use the supplied application-penetration-testing skill to answer this case.
> This is tabletop reasoning only. Read only the supplied case and skill files,
> and write one answer. Do not access networks, accounts, applications, other
> repositories, other answers or grading material. State your conclusion,
> evidence level, limits and next justified actions, plus tools actually used.

## Recorded result

All ten v1.1.0 answers received full passes in the independent grading. Exact
model ID/effort was not exposed by the delegation interface; inherited Codex
settings were used without an override. The grader could see the version label.
Tool use was self-reported, not independently traced.

The set was derived from known audit lessons, with one sample per case and no
zero-skill control or held-out set. It does not establish general reliability,
statistical significance, real-world execution quality or production readiness.
Public answers also make these cases unsuitable as an unseen benchmark for
agents that have already read this repository. Add new independent cases for
stronger future evaluation.

# Master's Thesis Timeline: September 2026-May 2027
## RIT's Schedule Guidelines

- RIT's [2026-2027 Academic Calendar](https://www.rit.edu/calendar) places the last Fall 2026 class day on December 7, Spring 2027 classes from January 11 through April 26, final exams from April 28 through May 5, and commencement on May 7-8.
- RIT's [general master's thesis milestones](https://www.rit.edu/liberalarts/sites/rit.edu.liberalarts/files/documents/MS-Masters-Thesis-Guidelines%20%28Revised%202017%29%208_21_17.pdf) proceed from topic and committee formation through proposal approval, regular sharing of results and chapter drafts, final defense, revisions, and repository submission.
## Milestone summary

| Month | Primary focus | Exit milestone |
|---|---|---|
| September 2026 | Establish the training baseline | Reproducible baseline policy and training record |
| October 2026 | Define the general hardware specification | Advisor-approved hardware specification v1 and experiment plan |
| November 2026 | Implement the specification in simulation | Verified parameterized model and first robustness results |
| December 2026 | Complete the main experiments | Frozen main dataset, analysis, and reproducibility package |
| January 2027 | Establish the thesis draft | Introduction, background, and methods draft |
| February 2027 | Write results and close evidence gaps | Complete rough draft and final small experiments |
| March 2027 | Produce the committee-ready thesis | Full thesis delivered to the committee |
| April 2027 | Defend and revise | Successful defense and committee-approved corrections |
| May 2027 | Complete submission and graduate | Accepted thesis, archived artifacts, and degree requirements complete |

## Detailed monthly plan

### September 2026 - Baseline and infrastructure

**Status:** Substantially completed.

- [x] Create an initial quadruped model in MuJoCo.
- [x] Move initial Stable-Baselines3 work to RSL-RL for GPU training.
- [x] Move the simulation workflow from MuJoCo to MJLab.
- [x] Develop an initial reward function and begin training on RC resources.
- [ ] Preserve a reproducible baseline before changing the model:
  - Record the simulator, model, observation, action, reward, and training configurations.
  - Save at least one baseline checkpoint and its evaluation results.
  - Record software versions, random seeds, commands, and hardware used.
  - Summarize the baseline's expected behavior and known failure modes.

**Exit milestone:** A baseline run can be repeated and compared fairly with every later policy.

### October 2026 - Define the general hardware specification

**Goal:** Convert "generally defined hardware" into a precise, testable contract.

- [ ] Write hardware specification v1, including:
  - Morphology: topology, link dimensions, mass, center of mass, inertia, joint axes, and joint limits.
  - Actuation: torque and speed limits, control mode, command limits, control frequency, delay, and actuator uncertainty.
  - Sensing and policy observations: joint states, IMU or body state, unavailable privileged information, update rates, delay, noise, and dropout assumptions.
  - Contact and environment assumptions: friction, restitution, terrain, payload, external disturbance, and initial-state ranges.
  - Policy interface: fixed or structured observation and action representation, normalization, masking, and any hardware descriptors given to the policy.
- [ ] Separate parameters into three sets:
  - **Nominal:** the reference robot used for debugging and baseline comparison.
  - **In-distribution:** the hardware range represented during training.
  - **Out-of-distribution:** held-out hardware used only to test generalization.
- [ ] Define quantitative robustness metrics before running the main experiments, such as task success, return, fall rate, tracking error, energy or torque use, and performance degradation from nominal.
- [ ] Define the comparison set: nominal-only policy, randomized/generalized policy, and the proposed structured policy.
- [ ] Create the experiment matrix, required random seeds, evaluation episodes, statistical summaries, and failure criteria.
- [ ] Review the specification and experiment plan with the advisor and thesis committee or intended committee.

**Target date:** October 31, 2026.

**Exit milestone:** The advisor agrees that the specification, research question, baselines, metrics, and experiment matrix are sufficient to support the thesis claim.

### November 2026 - Simulation model and first robustness study

**Goal:** Reproduce the hardware specification in simulation and implement the newer structured model.

- [ ] Implement a parameterized simulation generator that maps the hardware specification to valid robot instances.
- [ ] Add automated checks for dimensions, mass and inertia, joint ordering, limits, observation/action shapes, and actuator configuration.
- [ ] Validate several boundary models from the in-distribution range visually and numerically.
- [ ] Implement the structured model or policy representation that consumes the general hardware definition.
- [ ] Freeze evaluation code and common training settings so every method is tested identically.
- [ ] Train the nominal baseline, the generalized/randomized baseline, and the proposed structured policy.
- [ ] Run a small pilot robustness sweep across nominal, in-distribution, and held-out hardware.
- [ ] Use the pilot only to fix implementation defects or underpowered experiment design; document every change to the October plan.

**Target dates:**

- November 13: parameterized simulator and validation checks working.
- November 20: structured model and all baselines training end to end.
- November 30: pilot sweep reviewed with the advisor and main experiment plan frozen.

**Exit milestone:** All methods run through the same verified pipeline and produce interpretable pilot results.

### December 2026 - Main experiment completion and research freeze

**Goal:** Finish the evidence needed for the core thesis claim during the fall semester.

- [ ] Run the full experiment matrix using the predetermined seeds and evaluation protocol.
- [ ] Complete the primary comparisons across nominal, in-distribution, and out-of-distribution hardware.
- [ ] Complete the planned ablations needed to identify which parts of the structured model contribute to robustness.
- [ ] Perform stress tests for the most important hardware, sensing, actuation, and contact variations.
- [ ] Calculate uncertainty or statistical summaries and inspect failed runs rather than reporting only averages.
- [ ] Generate versioned tables and figures directly from saved results.
- [ ] Re-run a small representative subset from clean commands to verify reproducibility.
- [ ] Write a short fall research report containing the question, methods, completed matrix, primary results, limitations, and open evidence gaps.
- [ ] Back up code, configurations, checkpoints, raw results, processed results, and plotting scripts.

**Target dates:**

- December 7, the last day of fall classes: all planned main jobs launched or completed.
- December 16, the end of final exams: main experiment dataset frozen.

After the freeze, do not introduce a new core policy, simulator, or thesis claim unless the advisor agrees that the existing evidence cannot support a defensible thesis.

**Exit milestone:** The central thesis claim can be supported or rejected using a frozen, reproducible result set.

### January 2027 - Thesis structure and methods draft

**Goal:** Turn the completed fall work into the first half of the thesis.

- [ ] Confirm all program-specific defense, graduation, formatting, and submission requirements with the graduate program coordinator.
- [ ] Confirm the final thesis committee and reserve a tentative early-April defense window.
- [ ] Create the complete chapter outline and figure/table inventory.
- [ ] Draft the introduction, problem statement, contributions, and limitations.
- [ ] Complete the related-work review and clearly distinguish this work from domain randomization, morphology-conditioned policies, hardware-aware policies, and transfer methods.
- [ ] Draft the hardware specification, simulation, policy, training, and evaluation methodology chapters.
- [ ] Convert the experiment log into a traceable appendix or reproducibility record.
- [ ] Ask the advisor to review chapters as they become ready rather than waiting for a full draft.

**Exit milestone:** Introduction, background, and methods form a coherent advisor-reviewed draft, and the defense window is tentatively held.

### February 2027 - Results draft and small gap-closing experiments

**Goal:** Complete the full story without reopening the project scope.

- [ ] Draft the results chapter using the frozen December data.
- [ ] Draft the discussion: what generalized, what failed, why the structured representation helped or did not help, and what the simulator cannot establish about physical hardware.
- [ ] Make a numbered list of evidence gaps found during writing.
- [ ] Run only small experiments that answer one listed gap, such as:
  - One missing ablation.
  - A narrow sensitivity sweep.
  - Additional seeds for a high-variance result.
  - A targeted failure-case analysis.
  - A corrected evaluation whose flaw is documented.
- [ ] Update the figures, tables, captions, appendices, and reproducibility material.
- [ ] Write the conclusion, contributions, limitations, and future work.

**Scope rule:** A spring experiment should have one question, use the existing pipeline, and finish in roughly one to two working days. Discuss anything larger with the advisor before starting it.

**Target dates:**

- February 19: end of planned experimental work.
- February 26: complete rough draft sent to the advisor.

**Exit milestone:** A complete thesis exists, with no unwritten chapters and no experiment required for the central claim.

### March 2027 - Revision and committee-ready thesis

**Goal:** Move from a complete draft to a defensible document.

- [ ] Resolve the advisor's structural and technical feedback.
- [ ] Check that every research question maps to a method, result, and conclusion.
- [ ] Audit claims against the figures, tables, statistical summaries, and experiment records.
- [ ] Improve the discussion of limitations, sim-to-real uncertainty, and the boundary of the claimed hardware generality.
- [ ] Complete citations, notation, appendices, and formatting.
- [ ] Confirm the defense date, room or online arrangement, forms, and announcement process.
- [ ] Deliver the full committee-ready thesis at least four weeks before the planned defense unless the program specifies a different interval.

**Target dates:**

- March 12: committee-ready thesis submitted.
- March 26: committee feedback consolidated and defense announcement completed for an April 9 target defense, subject to program rules.

**Exit milestone:** The committee has the complete document and the defense is officially scheduled.

### April 2027 - Defense, corrections, and final submission

**Goal:** Defend early enough to preserve time for corrections and submission review.

- [ ] Apply to graduate by April 1 to be included in the commencement book; verify completion in SIS.
- [ ] Build a defense presentation centered on the problem, hardware specification, proposed model, experiment design, results, limitations, and contribution.
- [ ] Rehearse with the advisor, then conduct at least one timed practice with a technical audience.
- [ ] Prepare backup slides for model details, ablations, statistics, and failure cases.
- [ ] Defend the thesis, targeting April 9 and no later than April 16 if committee availability permits.
- [ ] Record every required correction and identify who must approve it.
- [ ] Complete committee corrections and obtain final approval.
- [ ] Submit the approved thesis through the program's required RIT repository or ProQuest process and resolve formatting or metadata corrections.
- [ ] Complete the spring graduation application no later than April 26, RIT's published final deadline for Spring 2027 graduation.

**Exit milestone:** The thesis is defended, corrected, approved, and submitted before final-exam week whenever possible.

### May 2027 - Acceptance, archive, and program completion

**Goal:** Close every administrative and reproducibility task before leaving the program.

- [ ] Verify that the final thesis submission has been accepted and that no thesis or graduation hold remains.
- [ ] Confirm degree-completion status with the advisor and graduate program coordinator.
- [ ] Archive the final code, exact configurations, environment or dependency lock files, model definitions, checkpoints, raw results, analysis scripts, and final figures.
- [ ] Add a README with exact reproduction and evaluation commands plus known hardware or compute requirements.
- [ ] Preserve only data and artifacts that may be shared under RIT, lab, sponsor, and licensing rules.
- [ ] Attend convocation/commencement on May 7-8, 2027, if participating.

**Exit milestone:** All degree requirements are complete and the thesis artifacts are understandable and reproducible by someone other than the author.

## Standing weekly cadence

- **Monday:** Select one measurable research or writing deliverable for the week.
- **During experiments:** Log the hypothesis, exact configuration, code revision, seed, output location, result, and interpretation for every run.
- **Weekly:** Send the advisor a concise progress update containing completed work, current evidence, blockers, and the next deliverable.
- **Every two weeks:** Review scope, results, and thesis claims with the advisor; share current figures or prose rather than a verbal update alone.
- **Friday:** Commit or back up work, update this checklist, and record the next action needed to resume without reconstructing context.
- **Monthly:** Produce a one-page checkpoint containing accomplishments, evidence, unresolved risks, and the next month's exit milestone.

## Completion gates

Do not advance a major claim merely because training finished. Use these gates:

1. **Specification gate:** Hardware ranges and policy inputs are explicit enough to generate and independently inspect a robot instance.
2. **Simulation gate:** Generated models satisfy the specification numerically and behave plausibly under basic tests.
3. **Experiment gate:** Baselines and the proposed method use the same evaluation protocol, seeds, metrics, and reporting rules.
4. **Evidence gate:** Results include variability, failure cases, and held-out hardware rather than only a favorable nominal example.
5. **Writing gate:** Every central claim points to reproducible evidence and states its limitations.
6. **Graduation gate:** Defense, revisions, repository acceptance, and program paperwork are all confirmed rather than assumed.


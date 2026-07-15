# SunnyPilot GV70 Project Instructions

## Scope and repositories

- `/Users/nick/sunnypilot` is the SunnyPilot parent repository.
- `/Users/nick/sunnypilot/opendbc_repo` is its OpenDBC submodule. Treat it as an independent repository.
- Expected parent branch: `gv70-2022-25t-hda2`.
- Expected OpenDBC branch: `gv70-2022-25t-hda2`.
- Known parent platform baseline: `27bcb4f6cb` (`Use GV70 HDA2 opendbc platform branch`).
- Known parent safety-pointer commit: `5de643a87e` (`Update OpenDBC GV70 safety fix`).
- Known OpenDBC baseline: `d013c0b8` (`Add 2022 Genesis GV70 2.5T HDA2 platform`).
- Before any change, report both branches, HEADs, worktree status, and the parent submodule pointer. Stop on unexpected state or detached HEAD.

## Change discipline

- Make one logical change per commit with minimal diffs and no unrelated refactoring.
- Test before committing. Run narrow tests before relevant broad suites and review the exact diff.
- Never weaken panda or vehicle safety, force `controls_allowed`, or enable longitudinal control.
- Do not change torque limits without explicit approval and a safety/test rationale.
- Do not modify or deploy to the comma unless deployment is explicitly approved.
- Prefer terminal Git. The human may use GitHub Desktop for visual review.
- Use the exact project Python interpreter or environment; do not use Python Launcher.
- Use Docker when it materially improves reproducibility.
- Do not commit or push unless explicitly requested or authorized by the confidence-gated council process below.

## Confidence-gated technical decision process

For every meaningful technical decision, use the `technical-strategy-council` skill. The council must generate exactly three genuinely different strategies, cross-review them, score them, synthesize the strongest recommendation, and assign a final **Recommendation Confidence Score** from 0 to 100. The score represents confidence that the recommended next milestone can be executed safely, not merely that its idea is interesting.

The council must identify assumptions, risks, validation gates, rollback, and stop conditions. Calculate the final score with this rubric:

| Criterion | Points |
| --- | ---: |
| Correctness and evidence | 25 |
| Safety and regression risk | 20 |
| Testability and validation quality | 15 |
| Reversibility and rollback quality | 10 |
| Fit with current repository state | 10 |
| Scope clarity and boundedness | 10 |
| Maintainability | 5 |
| Execution and deployment simplicity | 5 |
| **Total** | **100** |

Do not inflate the score to avoid requesting approval. Apply these thresholds:

- **85–100:** Proceed automatically with one bounded recommended milestone unless a mandatory approval condition applies.
- **70–84:** Stop, present the recommendation and score, explain why it did not reach 85, and ask the user to approve, reject, or revise it.
- **Below 70:** Do not recommend implementation. Present evidence gaps, unresolved risks, and the safest next diagnostic step for review.

A score of 85 or higher requires a clearly established root cause or objective; narrow scope; known affected files and systems; clear rollback; available required tests; understood repository state; no unresolved high-severity risk; no unverified assumption critical to safety; and scope within the approved project boundaries.

### Mandatory approval overrides

Regardless of score, always stop and ask the user before:

- deploying software to the comma
- road testing or vehicle movement
- changing panda safety behavior beyond an already-approved and tested fix
- enabling longitudinal control
- changing torque, steering, gas, brake, RX, TX, relay, or forwarding limits
- modifying the physical harness or vehicle wiring
- force-pushing, deleting branches, rewriting history, or deleting repository data
- exposing, changing, or creating credentials, API keys, or secrets
- installing unreviewed third-party executable code
- making a broad refactor across multiple vehicle platforms
- changing the agreed project scope
- proceeding when worktrees are unexpectedly dirty
- proceeding when tests fail
- proceeding when repository or device state differs from the council assumptions

With a score of 85 or higher and no override, the council may proceed automatically with bounded source changes, regression tests, local tests, read-only log analysis, documentation, non-destructive Docker work, ordinary commits on the approved feature branch, ordinary pushes to the approved personal remote, a parent submodule-pointer update after verifying the child commit, or creation/update of a draft pull request.

When proceeding automatically, briefly state the recommendation and score; execute only one bounded milestone; run all required validation; verify repository state afterward; and report files changed, diff summary, commands, tests, commit SHAs, and remaining risks. Run the council again before the next meaningful decision.

If new information lowers confidence below 85, stop immediately, do not improvise a larger solution, present the revised council result, and request confirmation.

Do not use the council for typo corrections, formatting-only changes, status reporting, already-approved tests, already-approved command sequences, or routine work inside an approved milestone when no new decision is required.

For SunnyPilot, OpenDBC, comma, CAN, panda, or vehicle work, this process never relaxes existing restrictions. Never force `controls_allowed`; never weaken panda safety; never bypass torque, steering, gas, brake, RX, TX, relay, or forwarding checks; never enable longitudinal without explicit approval; and never perform or recommend uncontrolled road testing. Require applicable tests, replay validation, clean repositories, recoverable backups, and parked-validation gates.

## Submodule release order

Commit and push OpenDBC first, verify the remote commit, then update and separately commit the parent submodule pointer. Never force push. Treat Git LFS uploads cautiously and explain any unexpectedly large inherited-object upload first.

## Documentation and diagrams

Keep GV70 evidence and session records under `docs/gv70/`. Separate confirmed observations from hypotheses and unknowns. Use fenced Mermaid diagrams for Markdown-native topology or workflow diagrams when they improve clarity; keep labels quoted when punctuation is present.

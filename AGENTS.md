# Universal Codex Baseline with SunnyPilot GV70 Overlay

## Universal baseline

The universal operating model applies throughout this repository. Project and nested instructions may add stricter requirements but may not weaken universal safety, security, privacy, approval, Git, validation, or rollback rules.

Apply instructions in this order: platform/system requirements, the current user request, this root file, applicable nested `AGENTS.md` files, approved specifications and decisions, applicable installed skills, then repository conventions supported by source and tests. Stop and report material conflicts.

Before meaningful work, inspect the applicable instructions, repository structure, manifests and lockfiles, build/test/lint configuration, documentation, CI, branches, remotes, worktrees, submodules, deployment configuration, rollback state, and current failures. Inspect unfamiliar command help instead of guessing.

Use deterministic repository-native terminal workflows. Prefer minimal diffs, one logical change per milestone and commit, test-first changes, systematic root-cause debugging, exact diff review, and explicit residual-risk reporting. Never claim validation that was not run. Do not commit when required validation fails.

Do not commit secrets, modify shell startup files, silently broaden permissions, weaken trust boundaries, or mix unrelated cleanup with requested work. Audit third-party skills, plugins, scripts, dependencies, models, and installers for provenance, license, executable behavior, hooks, network and credential access, destructive actions, telemetry, persistence, permissions, and conflicts before enabling them.

Use containers only when they materially improve reproducibility or isolation. Do not use privileged containers or mount sensitive host paths without explicit approval.

## Continuous milestone execution and reporting

For meaningful decisions use the global `technical-strategy-council` skill with all five permanent voting members:

1. Conservative Systems Engineer
2. Software and Systems Architect
3. Product and User Advocate
4. Quality, Security, and Operations Lead
5. Adversarial Risk Analyst

Specialist advisors may contribute without replacing or voting for a permanent member. Establish facts, distinguish inferences and unknowns, compare material options, cross-review them, synthesize one bounded milestone, and score that milestone independently.

Continue automatically through consecutive milestones scoring 85 or higher while validation passes, state remains expected, rollback is available, and no mandatory gate applies. Do not stop merely to report success. Maintain a running record and provide one consolidated synopsis at the first legitimate stopping point.

At 70–84, stop before implementation and request a decision. Below 70, do not implement; report the evidence gap and safest diagnostic step. Stop immediately if confidence falls below 85 during execution.

At a genuine user-decision stop, invoke:

```bash
~/.codex/bin/decision-required \
  "Codex Decision Required" \
  "<concise reason user input is required>" \
  "SunnyPilot GV70" \
  "<optional local report path>"
```

Never include phone numbers, account details, credentials, private source, or sensitive logs in notification arguments or repository files. Do not invoke the notifier for routine progress, successful milestones, informational reports, or decisions safely resolved under the autonomy rules.

## Universal mandatory approval gates

Regardless of score, stop before production deployment; destructive production migration; deletion of production or user data; billing, payment, subscription, or financial changes; irreversible external transactions, messages, orders, or submissions; authentication or authorization architecture changes; secret creation, exposure, rotation, transmission, or modification; granting private-account or personal-data access; making private data public; unapproved paid service use; force-pushing or rewriting published history; deleting branches, repositories, releases, or protected tags; merging into a protected production branch; weakening repository, sandbox, signing, or security controls; administrator or system-wide installation; persistent background services, startup items, drivers, kernel extensions, or unrestricted remote access; safety-critical hardware deployment; uncontrolled physical movement; material legal, compliance, privacy, or retention changes; major scope expansion; proceeding after required validation fails or state becomes unexpected; or proceeding without credible rollback.

The project overlay below adds stricter vehicle-specific gates. It never removes a universal gate.

## SunnyPilot GV70 project overlay

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

For every meaningful technical decision, use the five-member `technical-strategy-council` process above. Compare genuinely different options when alternatives exist; do not manufacture cosmetic strategies. The final **Recommendation Confidence Score** represents confidence that the next bounded milestone can be executed safely, not enthusiasm for the idea.

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

- **85–100:** Proceed automatically with the bounded recommended milestone and reassess the next milestone unless a mandatory approval condition applies.
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
- installing third-party executable code that does not satisfy the capability-acquisition requirements below
- making a broad refactor across multiple vehicle platforms
- changing the agreed project scope
- proceeding when worktrees are unexpectedly dirty
- proceeding when tests fail
- proceeding when repository or device state differs from the council assumptions

With a score of 85 or higher and no override, the council may proceed automatically with bounded source changes, regression tests, local tests, read-only log analysis, documentation, non-destructive Docker work, ordinary commits on the approved feature branch, ordinary pushes to the approved personal remote, a parent submodule-pointer update after verifying the child commit, or creation/update of a draft pull request.

When proceeding automatically, record the recommendation and score, execute only the scored bounded scope, run required validation, verify state afterward, and continue to the next independently scored milestone. Defer the consolidated report until a legitimate stop point.

If new information lowers confidence below 85, stop immediately, do not improvise a larger solution, present the revised council result, and request confirmation.

Do not use the council for typo corrections, formatting-only changes, status reporting, already-approved tests, already-approved command sequences, or routine work inside an approved milestone when no new decision is required.

For SunnyPilot, OpenDBC, comma, CAN, panda, or vehicle work, this process never relaxes existing restrictions. Never force `controls_allowed`; never weaken panda safety; never bypass torque, steering, gas, brake, RX, TX, relay, or forwarding checks; never enable longitudinal without explicit approval; and never perform or recommend uncontrolled road testing. Require applicable tests, replay validation, clean repositories, recoverable backups, and parked-validation gates.

## Capability acquisition and self-improvement

The technical-strategy-council may recommend acquiring or improving models, Codex skills, personas, scripts, agent definitions, plugins, MCP integrations, development dependencies, and other supporting capabilities when doing so materially improves efficiency, correctness, safety, or likelihood of success.

A Recommendation Confidence Score of 85–100 automatically authorizes one bounded capability-acquisition milestone when all of these conditions are satisfied:

- the capability is directly relevant to the approved project objective
- its exact source, publisher, version or commit, license, and installation scope are known
- it comes from an official registry, verified upstream repository, or otherwise reviewed source
- published signatures or checksums are verified when available
- executable code, installation hooks, requested permissions, network behavior, and dependency changes have been reviewed
- no unresolved high-severity vulnerability, provenance, licensing, privacy, or supply-chain risk remains
- installation is limited to the SunnyPilot workspace, Codex user configuration, an isolated project environment, or a disposable container
- no administrator privileges or system security-policy changes are required
- no credentials, secrets, private data, browser sessions, or external accounts are exposed or modified
- all created and modified files are known
- validation, rollback, and removal procedures are defined
- repositories are clean and their state matches council assumptions
- the acquisition does not modify the comma, vehicle, panda, firmware, harness, CAN behavior, or safety limits

Automatically authorized actions at 85–100 include:

- discovering and evaluating relevant capabilities
- installing or updating reviewed Codex skills and agent definitions
- creating or improving project-specific skills, personas, prompts, scripts, tests, and documentation
- installing reviewed project-local dependencies and plugins
- configuring reviewed MCP integrations without creating or exposing credentials
- downloading approved models into project-local or Codex-user storage when license, size, provenance, resource requirements, and rollback are understood
- committing and pushing resulting bounded changes to the approved personal branch
- disabling or removing an acquired capability if validation fails

Always require explicit approval before:

- administrator-privileged or system-wide installation
- installing unverifiable, unsigned, abandoned, or materially unreviewed executable code
- downloading a model whose license, provenance, size, or runtime requirements are unresolved
- creating, revealing, changing, or transmitting credentials or secrets
- granting access to private accounts, browser sessions, email, cloud storage, calendars, or other personal data
- enabling persistent background services, startup items, kernel extensions, drivers, or unrestricted remote access
- weakening sandboxing, code-signing, operating-system security, repository protections, or vehicle safety controls
- acquiring capabilities unrelated to the approved SunnyPilot objective
- allowing acquired agents or scripts to broaden their own permissions or recursively install additional capabilities without a fresh council evaluation

Capability acquisition must follow the same one-milestone rule. After installation, verify provenance, files changed, permissions, dependency or lockfile changes, tests, security findings, resource use, and rollback. If validation fails or confidence drops below 85, stop and report the revised recommendation.

## Submodule release order

Commit and push OpenDBC first, verify the remote commit, then update and separately commit the parent submodule pointer. Never force push. Treat Git LFS uploads cautiously and explain any unexpectedly large inherited-object upload first.

## Documentation and diagrams

Keep GV70 evidence and session records under `docs/gv70/`. Separate confirmed observations from hypotheses and unknowns. Use fenced Mermaid diagrams for Markdown-native topology or workflow diagrams when they improve clarity; keep labels quoted when punctuation is present.

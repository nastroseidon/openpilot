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
- Do not commit or push unless explicitly requested.

## Submodule release order

Commit and push OpenDBC first, verify the remote commit, then update and separately commit the parent submodule pointer. Never force push. Treat Git LFS uploads cautiously and explain any unexpectedly large inherited-object upload first.

## Documentation and diagrams

Keep GV70 evidence and session records under `docs/gv70/`. Separate confirmed observations from hypotheses and unknowns. Use fenced Mermaid diagrams for Markdown-native topology or workflow diagrams when they improve clarity; keep labels quoted when punctuation is present.

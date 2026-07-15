# GV70 Lateral-Only Deployment Gate

Date: 2026-07-15 (America/Indiana/Indianapolis)

Scope: installation readiness for stationary lateral validation only

Verdict: **GO, subject to explicit deployment approval and all just-in-time preconditions below**

This verdict authorizes no device change by itself. It does not authorize vehicle movement, a road test, longitudinal control, Experimental Mode, harness changes, or safety-limit changes. Commit `b1867e52c3d914361fd735557a5b0539d86326e9` remains locally and offline-replay verified, not vehicle-validated.

## Shared facts

### Confirmed

- SunnyPilot is clean on `gv70-2022-25t-hda2` at `479513cf33e6860739d9908de901295ee395de1f`; its personal remote is exact.
- OpenDBC is clean on `gv70-2022-25t-hda2` at `8da0abaf1a1e8d2e643a2a4528cbe18582fcb78a`; its personal remote is exact and the parent pointer matches.
- `b1867e52` is an ancestor of OpenDBC HEAD. Later child commits change only `AGENTS.md` and `docs/CARS.md`, not safety or vehicle-support source.
- The safety change is one SCC-bus expression plus a mixed-flag regression class. It does not change torque, gas, brake, TX, relay, forwarding, or longitudinal limits.
- Focused corrected tests passed 67 tests with 4 expected capability skips; the pre-fix negative control produced 7 expected authorization-path failures; the complete Hyundai CAN-FD module passed 2,031 tests with 160 expected skips.
- Corrected safety replay accepted 1,053,158 RX frames and 35,080 outgoing frames across four GV70 segments with zero invalid RX and zero blocked TX. Restoring only the old expression blocked 7,252 authorization-dependent messages and never authorized controls.
- The comma is currently clean on `dev` at `3f8e95952aa342cab21c232462afb0da88e2263e`, backed by `origin/dev` and both safe-staging trees.
- Cached CarParams identifies `GENESIS_GV70_2022_2_5T_HDA2`, `hyundaiCanfd`, safety parameter `0x18`, `pcmCruise=true`, and both longitudinal fields false. Alpha Longitudinal and Experimental Mode parameters are absent/disabled.

### Strong inferences

- Installing the reviewed parent/child pair should correct the confirmed SCC-bus authorization mismatch while preserving the existing lateral command limits.
- A stationary, ignition-on vehicle connection is sufficient to verify fingerprint, active panda safety configuration, health, controls authorization transitions, and rejected-transmit behavior without moving the vehicle.

### Unknowns

- The corrected build has not run on this comma or vehicle.
- Active connected panda state `hyundaiCanfd/0x18` and the exact flashed firmware signature have not yet been observed with the reviewed build.
- The offroad cumulative SPI error count was 3,132; its stability under a connected parked session is unknown.

### Decision required

Whether the reviewed branch is ready to be installed solely to perform a stationary lateral validation.

### Non-negotiable constraints

- Explicit user approval is required before deployment.
- No vehicle movement or road test.
- No longitudinal or Experimental Mode enablement.
- No panda safety weakening, harness change, forced authorization, or limit change.
- Stop on any repository, remote, device, parameter, firmware, or health mismatch.

### Success criteria

The exact reviewed software is installed recoverably; the vehicle remains parked; the expected GV70 fingerprint and `hyundaiCanfd/0x18` state appear; longitudinal remains false; panda and process health remain valid; and no unexplained event, invalid RX, rejected TX, heartbeat fault, or increasing SPI error pattern appears.

## Three strategies

### Strategy A — Minimal reviewed installation plus stationary validation

Install only the remotely verified feature branch after a fresh read-only preflight, preserve `3f8e9595` as rollback, and perform the checklist below without vehicle movement. Assumes the device can fetch the verified personal remote and that the branch builds normally. Validation is exact SHA/configuration/health/event inspection. Primary risks are installing the wrong ref or discovering a device-only fault; SHA checks and immediate rollback mitigate both. Rollback is the clean, remote-backed `dev` commit. Complexity: medium. Confidence: 9.5/10.

### Strategy B — Build an isolated deployable artifact before installation

Produce and checksum a device-compatible artifact in an isolated environment, then install that artifact for parked validation. This improves artifact provenance and reproducibility, but introduces a packaging path not yet evidenced for this branch and expands the milestone. Validation would add artifact-to-source reproducibility checks. Rollback remains `3f8e9595`. Complexity: high. Confidence: 8.2/10.

### Strategy C — Defer installation and collect more non-vehicle evidence

Do not deploy; repeat or expand replay, static analysis, and CI until device-side uncertainty is further reduced. This is maximally conservative but cannot observe the remaining active firmware, connected safety-mode, and process-integration questions. Rollback is unnecessary. Complexity: low, with diminishing evidence value. Confidence: 8.4/10.

## Cross-review

- A on B: artifact provenance is valuable, but an unproven packaging workflow adds more risk than it removes for this narrow feature branch. A on C: C preserves safety but cannot close the device-only evidence gap.
- B on A: A has the shortest path and strongest repository fit, but must record the installed SHA and firmware result before accepting the parked session. B on C: more replay cannot validate installation integration.
- C on A: A must treat every connected-state mismatch as an abort, not troubleshoot live by relaxing a gate. C on B: B broadens scope and could falsely imply artifact equivalence without a reproducible device build.

## Comparative scorecard

| Criterion | Strategy A | Strategy B | Strategy C |
| --- | ---: | ---: | ---: |
| Correctness | 10 | 9 | 8 |
| Safety | 9 | 9 | 10 |
| Evidence quality | 10 | 9 | 8 |
| Implementation risk | 9 | 7 | 10 |
| Testability | 10 | 9 | 7 |
| Rollback quality | 10 | 10 | 10 |
| Maintainability | 9 | 10 | 7 |
| Deployment simplicity | 9 | 6 | 10 |
| Time to validated result | 10 | 6 | 5 |
| Repository fit | 10 | 7 | 8 |
| **Total / 100** | **96** | **82** | **83** |

## Council recommendation

Use Strategy A. It is the only approach that closes the remaining device-integration gap with a bounded, reversible, stationary procedure, while relying on the already tested and remotely recoverable commits. Adopt B's artifact/SHA verification discipline and C's strict abort posture.

### Recommendation Confidence Score: 96/100

| Rubric criterion | Score |
| --- | ---: |
| Correctness and evidence | 25/25 |
| Safety and regression risk | 19/20 |
| Testability and validation quality | 15/15 |
| Reversibility and rollback quality | 10/10 |
| Fit with current repository state | 10/10 |
| Scope clarity and boundedness | 10/10 |
| Maintainability | 4/5 |
| Execution and deployment simplicity | 3/5 |
| **Total** | **96/100** |

The residual four points reflect the absence of corrected-build device execution, exact flashed-signature evidence, and connected parked panda evidence. Those are the purpose of the next post-install validation, not reasons to expand source changes.

## Just-in-time deployment preconditions

All must be reconfirmed immediately before any modification:

- [ ] Explicit deployment approval is recorded.
- [ ] Vehicle is securely parked, parking brake applied, transmission in Park, with no instruction to move it.
- [ ] Parent and OpenDBC branches, HEADs, remotes, clean worktrees, and submodule pointer exactly match the SHAs above.
- [ ] Device remains clean at `3f8e9595`; rollback remote and both safe-staging copies remain available.
- [ ] Target remote branch resolves exactly to parent `479513cf33` and child `8da0abaf`.
- [ ] `b1867e52` remains in child history and no later safety/vehicle-support diff exists.
- [ ] Alpha Longitudinal and Experimental Mode remain disabled; no write is made to enable either.
- [ ] The exact deployment command sequence is reviewed immediately before execution and contains no reset, history rewrite, unrelated cleanup, or parameter write.

Failure of any precondition changes this verdict to **NO-GO**.

## Stationary lateral-validation checklist

The deployment operation and any required service restart are separate, explicitly approved actions. After the device returns normally, keep the vehicle stationary throughout:

1. Record installed SunnyPilot branch and exact HEAD; require `gv70-2022-25t-hda2` and the approved parent SHA.
2. Verify the installed OpenDBC content corresponds to `8da0abaf` and contains `b1867e52`.
3. Verify the installed repository has no unexplained local modifications.
4. Reconfirm Alpha Longitudinal and Experimental Mode are disabled.
5. With ignition on and harness connected, require fingerprint `GENESIS_GV70_2022_2_5T_HDA2`.
6. Require `alphaLongitudinalAvailable=false`, `openpilotLongitudinalControl=false`, `pcmCruise=true`, and `radarUnavailable=false`.
7. Require active panda safety model `hyundaiCanfd` and parameter `0x18`; longitudinal authorization must remain false.
8. Record expected and observed panda firmware/signature state; require no firmware-out-of-date condition.
9. Require valid panda state, no faults, no heartbeat loss, no RX/TX overflow, no safety RX invalidity, and no unexplained blocked TX.
10. Observe SPI error count over a bounded stationary interval; require no persistent or rapid increase.
11. Exercise only stationary control-state transitions that require no steering-force test and no vehicle movement; verify no `controlsMismatch` or unexpected disengagement/event appears.
12. Preserve timestamped logs and exact CarParams/pandaStates/events evidence for review before considering any later milestone.

Passing this checklist establishes parked integration only. It does not establish safe operation in motion.

## Immediate abort criteria

Abort and restore the known-good `dev` target if safely possible when any of the following occurs:

- wrong branch, SHA, submodule content, dirty device tree, or target remote mismatch;
- failure to build, boot, start manager/pandad, or return to a stable offroad state;
- unexpected fingerprint, safety model, safety parameter, bus configuration, or firmware mismatch;
- either longitudinal field or Experimental Mode becomes enabled;
- panda fault, heartbeat loss, RX invalidity, buffer overflow, unexplained blocked TX, or persistent SPI-error growth;
- `controlsMismatch`, repeated process crash, unexpected relay/forwarding behavior, or unexplained vehicle warning;
- any need to weaken a safety check, change a limit, alter the harness, move the vehicle, or improvise beyond the reviewed sequence.

After abort, do not troubleshoot by bypassing protections. Record evidence, return to the clean rollback target, verify longitudinal remains disabled, and issue a new council review.

## Rollback target

Primary target: SunnyPilot `dev` at `3f8e95952aa342cab21c232462afb0da88e2263e`, verified in `origin/dev`, `/data/safe_staging/finalized`, and `/data/safe_staging/merged`. Rollback success requires a clean tree, normal manager/pandad startup, longitudinal and Experimental Mode disabled, and healthy offroad panda state. Rollback does not authorize vehicle movement.

## Final gate verdict

**GO for installation solely to perform lateral-only parked validation, after explicit deployment approval.**

**NO-GO for vehicle movement, road validation, longitudinal control, Experimental Mode, torque/steering/gas/brake/safety-limit changes, or any claim of vehicle-validated resolution.**

## Exact next milestone

After explicit approval: perform one controlled deployment of the exact remotely verified parent/child pair, then execute only the stationary checklist above. Stop immediately on any mismatch and report results before proposing another milestone.

# GV70 Test Plan

## Gate sequence

1. Record branches, SHAs, parent pointer, and clean/dirty state in both repositories.
2. Run the focused mixed-flag regression class and preserve the exact command/output.
3. Run the complete `opendbc/safety/tests/test_hyundai_canfd.py` suite.
4. Run any broader OpenDBC/SunnyPilot tests required by the exact diff.
5. Replay a named route and verify CarParams, events, pandaStates, `controlsAllowed`, rejected TX, CAN buses, and timestamps.
6. Review the exact diff for unrelated scope, longitudinal enablement, safety weakening, and torque-limit changes.
7. Only after all automated gates pass, perform parked validation under [DEPLOYMENT.md](DEPLOYMENT.md).

## Current evidence

`b1867e52` contains `TestHyundaiCanfdLKASteeringCameraSCC`, configured with powertrain and SCC bus 1. This setup task did not execute safety or replay tests and makes no claim that the commit passes them.

## Acceptance criteria

- Focused regression passes and would fail against the pre-fix safety hook for the intended reason.
- Complete Hyundai CAN-FD safety tests pass with no skips newly introduced by the change.
- Replay shows expected safety state transitions and no unexplained rejected TX.
- Both worktrees and the parent pointer are in the reviewed state.
- No longitudinal-control or torque-limit behavior changes.

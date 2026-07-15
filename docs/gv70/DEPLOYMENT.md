# GV70 Deployment and Validation Gate

Deployment requires explicit approval after a written `GO` decision. A passing build alone is not approval.

## Required before deployment

- Clean SunnyPilot and OpenDBC worktrees on the expected branches.
- Recorded branch backups and exact recoverable SHAs.
- Focused regression, complete Hyundai CAN-FD safety suite, and relevant unit tests passing.
- Replay validation passing on identified routes.
- Exact diff reviewed; no safety bypass, torque-limit change, or longitudinal enablement.
- OpenDBC commit pushed and remotely verified before the parent pointer is updated.

## Validation order

1. Static/bench checks with no vehicle motion.
2. Parked vehicle validation: correct fingerprint, expected CarParams/safety flags, healthy pandaStates, expected buses, and no unexplained events or rejected TX.
3. Only with a qualified human driver, explicit approval, a controlled environment, an observer/logging plan, and a defined abort procedure may limited road validation be considered.

Never conduct or recommend an uncontrolled road test. Never bypass a failed gate. Analysis-only work must not alter the comma.

# Controls Mismatch Root-Cause Analysis

## Confirmed code-level issue

Local OpenDBC commit `b1867e52` states that the mixed `CANFD_LKA_STEER_MSG | CAMERA_SCC` safety configuration expects both powertrain data and `SCC_CONTROL` on bus 1. Before that commit, `hyundai_canfd_rx_hook()` selected SCC bus 2 whenever `CAMERA_SCC` was set, even when LKA steering selected powertrain bus 1.

The commit changes SCC bus selection to prefer bus 1 when `hyundai_canfd_lka_steer_msg` is true and adds `TestHyundaiCanfdLKASteeringCameraSCC` with `PT_BUS = 1` and `SCC_BUS = 1`.

## Evidence closure

The root cause is confirmed for four previously collected failed segments. The evidence-closure milestone correlated:

- platform `GENESIS_GV70_2022_2_5T_HDA2`, safety model `hyundaiCanfd`, and `safetyParam = 0x18`;
- `SCC_CONTROL` (`0x1A0`) received on bus 1 / ECAN and stock LKA steering (`0x50`) on bus 2 / CAM;
- `controlsAllowed` remaining false;
- rejected outgoing `0x50` messages beginning 16 ms after `pcmEnable`; and
- `controlsMismatch` occurring 1.999 seconds after `pcmEnable`, consistent with the 200-cycle mismatch threshold at the 100 Hz controls rate.

The complete causal chain is therefore: the pre-fix safety hook selected bus 2 for SCC under the mixed flags, missed valid cruise state on bus 1, left panda authorization false, rejected steering after controls-side enablement, and caused `selfdrived` to raise `controlsMismatch` at its threshold.

## Local correction validation

Commit `b1867e52` is locally verified but not vehicle-validated.

- Corrected focused class: 67 tests passed, including 4 expected capability skips.
- Pre-fix negative control: 7 predicted authorization-path failures, 56 passes, and 4 expected skips after restoring only the old SCC-bus expression in a disposable copy.
- Complete Hyundai CAN-FD module: 2,031 tests passed, including 160 expected skips.

No torque limit, longitudinal behavior, forced authorization, or safety bypass changed. The remaining validation gap is replay/post-fix vehicle evidence; no deployment or vehicle validation has occurred. Continue to describe `b1867e52` as a locally verified correction, not a vehicle-validated resolution.

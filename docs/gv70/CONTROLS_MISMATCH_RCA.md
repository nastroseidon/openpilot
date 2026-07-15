# Controls Mismatch Root-Cause Analysis

## Confirmed code-level issue

Local OpenDBC commit `b1867e52` states that the mixed `CANFD_LKA_STEER_MSG | CAMERA_SCC` safety configuration expects both powertrain data and `SCC_CONTROL` on bus 1. Before that commit, `hyundai_canfd_rx_hook()` selected SCC bus 2 whenever `CAMERA_SCC` was set, even when LKA steering selected powertrain bus 1.

The commit changes SCC bus selection to prefer bus 1 when `hyundai_canfd_lka_steer_msg` is true and adds `TestHyundaiCanfdLKASteeringCameraSCC` with `PT_BUS = 1` and `SCC_BUS = 1`.

## Hypothesis boundary

That mismatch is a plausible cause of missing cruise-state authorization in panda safety for this flag combination, but the repository contains no timestamped device log proving the complete causal chain. There is not yet documented evidence correlating CarParams flags, received `SCC_CONTROL`, pandaStates/`controlsAllowed`, rejected TX, and user-visible events on the same route.

## Evidence required to close the RCA

1. Capture exact CarParams and safetyParam flags.
2. Confirm `SCC_CONTROL` address, bus, frequency, and timestamps.
3. Correlate pandaStates and `controlsAllowed` transitions.
4. Identify rejected TX and relevant events at the same timestamps.
5. Run the focused regression and complete Hyundai CAN-FD safety suite.
6. Perform replay validation using the identified route.

Until these are recorded, describe `b1867e52` as a tested code-level correction, not a vehicle-validated resolution.

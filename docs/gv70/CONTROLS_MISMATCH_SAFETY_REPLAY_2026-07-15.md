# GV70 Controls Mismatch Safety Replay

Date: 2026-07-15 (America/Indiana/Indianapolis)
Milestone: Approved read-only/offline replay validation
Disposition: **Corrected panda safety hook passes all four recorded segments; pre-fix negative control reproduces the authorization failure**

## Scope and safety boundary

This milestone copied four previously identified `rlog.zst` files from the comma into `/tmp` and replayed their recorded incoming and outgoing CAN through OpenDBC's maintained `safety_replay` harness. The comma inventory and copies were read-only.

No software was deployed. No comma file, service, setting, branch, or running process was modified. The vehicle was not operated. All OpenDBC builds and the pre-fix negative control were confined to disposable trees under `/tmp/gv70-replay-20260715`.

## Repository state

| Repository | Branch | HEAD before replay | Worktree |
| --- | --- | --- | --- |
| SunnyPilot | `gv70-2022-25t-hda2` | `72a7a1b6301fb4cf8eba7b4021ebadd664bed6e9` | Clean; one commit ahead of `gv70/gv70-2022-25t-hda2` |
| OpenDBC | `gv70-2022-25t-hda2` | `3c68b2e2b3b77635d0e1b7aa2be5361834f9822e` | Clean |

The parent submodule pointer exactly matched OpenDBC `3c68b2e2b3b77635d0e1b7aa2be5361834f9822e`. Corrective commit `b1867e52c3d914361fd735557a5b0539d86326e9` is an ancestor of that OpenDBC HEAD.

## Log provenance

The comma reported device time `2026-07-15T19:40:57+00:00` during the inventory.

| Segment | Device timestamp (UTC) | Bytes | SHA-256 |
| --- | --- | ---: | --- |
| `00000521--3eb261642e--1` | `2026-07-07 23:17:34.359002463` | 12,799,868 | `ad4582800ee79600d0f64df77bee7b09453814a805edd2bd09ee3f93ae431e65` |
| `00000521--3eb261642e--2` | `2026-07-07 23:18:34.359002441` | 12,847,636 | `766ef93b364564d61878ab20eebe03984ae29e262c543b7b898e4143bb06764b` |
| `00000522--8308e34b16--1` | `2026-07-08 00:39:11.199000595` | 13,057,456 | `0f28ddce4f5ff6d4d1136809bdcbf9739f40516c1c9dcc277be14890b2c58752` |
| `00000523--c75b32b9fb--1` | `2026-07-08 00:57:38.989000173` | 12,822,333 | `ac355f703881b90b2a11c904b73c3bc54eb56b7afac0344e8385b98bc22439cc` |

The local copies matched all four device hashes exactly.

## Replay method

The maintained harness was:

```text
opendbc/safety/tests/safety_replay/replay_drive.py
```

It sorted recorded `can` and `sendcan` events by monotonic time, initialized the safety mode from `CarParams`, fed incoming frames through `safety_rx_hook`, fed recorded outgoing frames through `safety_tx_hook`, and checked safety configuration validity throughout each segment.

Every segment resolved to:

- safety model: `hyundaiCanfd` (`28`)
- safety parameter: `24` / `0x18`
- alternative experience: `1024` (MADS enabled)
- SunnyPilot safety parameter: `2`

The corrected disposable tree was an exact source copy of OpenDBC HEAD. The negative-control tree changed only this expression in `opendbc/safety/modes/hyundai_canfd.h`:

```c
const unsigned int scc_bus = hyundai_camera_scc ? 2U : pt_bus;
```

That restores the SCC-bus selection from before `b1867e52` without changing any repository file.

## Corrected replay results

| Segment | RX frames | Invalid RX | Safety-config invalid | Outgoing frames | Blocked TX | MADS mismatch |
| --- | ---: | ---: | --- | ---: | ---: | ---: |
| `00000521--3eb261642e--1` | 263,232 | 0 | No | 7,420 | 0 | 0 |
| `00000521--3eb261642e--2` | 263,377 | 0 | No | 9,520 | 0 | 0 |
| `00000522--8308e34b16--1` | 263,422 | 0 | No | 8,900 | 0 | 0 |
| `00000523--c75b32b9fb--1` | 263,127 | 0 | No | 9,240 | 0 | 0 |
| **Total** | **1,053,158** | **0** | **No** | **35,080** | **0** | **0** |

The corrected hook entered authorized states in every segment. Recorded outgoing messages were evaluated while `controls_allowed` was true 17,177 times and while lateral authorization was true 21,523 times. No outgoing message was blocked.

## Pre-fix negative-control results

| Segment | Authorized outgoing evaluations | Blocked `0x50` steering | Blocked `0x1CF` buttons | Total blocked |
| --- | ---: | ---: | ---: | ---: |
| `00000521--3eb261642e--1` | 0 | 200 | 220 | 420 |
| `00000521--3eb261642e--2` | 0 | 400 | 2,320 | 2,720 |
| `00000522--8308e34b16--1` | 0 | 172 | 1,700 | 1,872 |
| `00000523--c75b32b9fb--1` | 0 | 200 | 2,040 | 2,240 |
| **Total** | **0** | **972** | **6,280** | **7,252** |

The negative control produced zero invalid incoming frames, but it never reached an authorized controls state and blocked the predicted authorization-dependent outgoing messages. The steering rejections include the approximately 200-cycle patterns previously correlated with `controlsMismatch`.

## Findings

### Confirmed

- The recorded logs are intact and reproducible from exact hashes.
- All four logs contain safety model `hyundaiCanfd` with mixed parameter `0x18`.
- Corrected OpenDBC accepts every recorded incoming frame and every recorded outgoing frame in all four segments.
- Restoring only the pre-fix SCC-bus expression prevents authorization and reproduces steering/button rejection in all four segments.
- The replay independently distinguishes corrected from pre-fix behavior using real GV70 CAN traffic.

### Safety invariants preserved

- No forced `controls_allowed` was added to production code.
- No RX, TX, relay, forwarding, steering-torque, gas, or brake limit was weakened.
- No longitudinal control was enabled.
- No repository source was modified for the negative control.

### Limitations and unknowns

- This is panda safety-hook replay, not a complete regeneration of `pandad`, `selfdrived`, or user-interface events. Original `pandaStates` and `controlsMismatch` events remain historical inputs; the harness does not synthesize new post-fix events.
- Replay success is not deployment success and is not vehicle validation.
- The corrected build has not been installed on the comma.
- Parked validation of fingerprint, safety configuration, panda state, authorization transitions, and rejected TX remains required after any separately approved deployment.

## Gate result

**Offline panda safety replay: PASS.**

**Deployment/vehicle validation: NO-GO pending a separate confidence-gated deployment decision and explicit user approval.**

The safest next milestone is a written deployment-readiness review that verifies recoverable branch/device rollback targets and defines exact static and parked acceptance/abort criteria. It must stop for explicit approval before modifying the comma.

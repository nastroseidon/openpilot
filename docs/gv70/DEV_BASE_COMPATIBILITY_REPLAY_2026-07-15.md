# GV70 Dev-Base Compatibility Replay

Date: 2026-07-15 (America/Indiana/Indianapolis)

Milestone: Offline safety replay of the minimal GV70 compatibility port based on the comma's installed SunnyPilot release

Disposition: **PASS**

## Scope and safety boundary

The replay used the four previously copied, hash-verified GV70 `rlog.zst` segments under `/tmp/gv70-replay-20260715`. It ran entirely on the Mac against the isolated compatibility worktree.

No comma file, branch, service, setting, firmware, or process was contacted or changed. No vehicle operation occurred. No longitudinal setting, Experimental Mode, panda limit, torque limit, or safety authorization rule was enabled or weakened.

## Tested source

| Item | Value |
| --- | --- |
| SunnyPilot compatibility branch | `codex/gv70-dev-base-compat` |
| Compatibility commit | `4ccbd2797445709db490dc88fa87aafd8fd6c3cb` |
| Exact base | `3f8e95952aa342cab21c232462afb0da88e2263e` |
| Base relationship | Exact commit currently installed on the comma during the read-only inventory |
| Source scope | Nine vendored OpenDBC files; 53 insertions, 9 deletions |

Before replay, every affected compatibility file was verified byte-for-byte identical to the corresponding reviewed OpenDBC content at `8da0abaf1a1e8d2e643a2a4528cbe18582fcb78a`.

## Prior automated gates on the compatibility branch

- Focused pre-fix RED control: 67 tests run, 7 expected authorization-path failures, 4 expected skips.
- Focused corrected GREEN test: 67 passed, 4 expected skips.
- Complete Hyundai CAN-FD safety module: 2,031 passed, 160 expected skips.
- Hyundai platform module: 13 passed, 2 expected skips.

## Replay command

From `/tmp/sunnypilot-gv70-dev-base`, once per segment:

```text
PYTHONPATH=/tmp/sunnypilot-gv70-dev-base:/tmp/gv70-test-deps:/Users/nick/Library/Python/3.12/lib/python/site-packages \
/Users/nick/.cache/codex-runtimes/codex-primary-runtime/dependencies/python/bin/python3 \
/tmp/sunnypilot-gv70-dev-base/opendbc_repo/opendbc/safety/tests/safety_replay/replay_drive.py \
<segment-rlog.zst>
```

Each segment resolved from its recorded CarParams to safety model `hyundaiCanfd` (`28`), safety parameter `0x18` (`24`), alternative experience `1024`, and SunnyPilot safety parameter `2`.

## Results

| Segment | RX frames | Invalid RX | Safety-config invalid | Outgoing frames | Blocked TX | MADS mismatch |
| --- | ---: | ---: | --- | ---: | ---: | ---: |
| `00000521--3eb261642e--1` | 263,232 | 0 | No | 7,420 | 0 | 0 |
| `00000521--3eb261642e--2` | 263,377 | 0 | No | 9,520 | 0 | 0 |
| `00000522--8308e34b16--1` | 263,422 | 0 | No | 8,900 | 0 | 0 |
| `00000523--c75b32b9fb--1` | 263,127 | 0 | No | 9,240 | 0 | 0 |
| **Total** | **1,053,158** | **0** | **No** | **35,080** | **0** | **0** |

The recorded outgoing evaluations occurred with `controls_allowed` true 17,177 times and lateral authorization true 21,523 times. None was blocked.

The replay script prints an internal `longitudinal_tx_allowed` getter reflecting instantaneous panda authorization and pedal state; it was true at the final sample of one segment. This is not evidence that openpilot longitudinal control was enabled or that gas/brake commands were sent. The tested platform remains lateral-only, recorded CarParams have `openpilotLongitudinalControl=false`, and this port changes no longitudinal setting, command, allowlist, or limit.

## Findings

### Confirmed

- The minimal compatibility port preserves the corrected replay behavior on the exact SunnyPilot base installed on the comma.
- All four recorded segments have zero invalid incoming frames, zero invalid safety-configuration intervals, zero blocked outgoing frames, and zero MADS mismatch.
- The result matches the previously reviewed submodule-based replay totals exactly.
- The compatibility port avoids the unrelated 18,183-commit development-history transition identified during deployment preflight.
- The compatibility branch is pushed and remotely verified at `4ccbd2797445709db490dc88fa87aafd8fd6c3cb` on `gv70/codex/gv70-dev-base-compat`.

### Remaining gaps

- The corrected compatibility build has not been installed or run on the comma.
- Active connected panda state, exact flashed firmware state, and stationary vehicle integration remain unverified.
- Replay success is not deployment success and is not vehicle validation.

## Gate result

**Offline compatibility replay: PASS.**

**Deployment: not authorized by this report.** A fresh deployment-readiness review must bind the compatibility commit, remote recoverability, exact artifact or fetch path, rollback target, and stationary acceptance/abort criteria before any device modification.

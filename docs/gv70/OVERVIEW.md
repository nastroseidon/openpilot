# Genesis GV70 2.5T HDA II Project Overview

## Confirmed scope

This work targets a 2022 Genesis GV70 2.5T with Highway Driving Assist II in the SunnyPilot parent repository and its OpenDBC submodule. OpenDBC commit `d013c0b8a7437ab4fde9a6d84eef096bb6439ffb` introduced `GENESIS_GV70_2022_2_5T_HDA2`; SunnyPilot commit `27bcb4f6cb4203153473fbebb8b50c189c4a6cd1` first updated the submodule pointer to it. Parent commit `5de643a87e73bf60f7c6abcf091efff1a012e048` subsequently advanced the recorded pointer to safety-fix commit `b1867e52c3d914361fd735557a5b0539d86326e9`.

The platform reuses the existing GV70 physical specs and torque-data values, identifies a distinct camera and radar firmware pair, and flags both CAN-FD radar SCC and camera SCC. A platform-specific helper places `SCC_CONTROL` parsing on ECAN for this mixed topology.

## Current repository state at setup

- Parent branch/HEAD before committing this documentation: `gv70-2022-25t-hda2` at `5de643a87e`.
- OpenDBC branch/HEAD: `gv70-2022-25t-hda2` at `b1867e52`.
- At the first inspection the local OpenDBC branch was one commit ahead of upstream. Its remote-tracking reflog later recorded an external `update by push` at 2026-07-14 22:33:17 -0400, and final verification showed `gv70/gv70-2022-25t-hda2` at `b1867e52`.
- Parent commit `5de643a87e` records OpenDBC `b1867e52`. Any later dirty submodule state must be evaluated against that recorded pointer rather than assumed to be the safety fix itself.

No deployment or vehicle-validation result is documented by repository history. See [TEST_PLAN.md](TEST_PLAN.md) and [DEPLOYMENT.md](DEPLOYMENT.md) before any such work.

## Safety invariants

Longitudinal control remains out of scope. Panda safety, normal control-authorization transitions, and existing torque limits must not be weakened. Every safety behavior change requires a focused regression test and the complete relevant Hyundai CAN-FD safety suite.

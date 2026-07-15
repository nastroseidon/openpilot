# GV70 Comma Read-Only Preflight

Date: 2026-07-15 (America/Indiana/Indianapolis)
Device time during inventory: `2026-07-15T20:41:12+00:00`
Milestone: Read-only comma inventory before lateral deployment decision

## Safety boundary

The comma was inspected through read-only SSH commands. No device file, parameter, branch, service, process, panda firmware, or vehicle state was changed. No deployment, restart, hardware query, or vehicle operation occurred.

## Installed software

| Item | Observed state |
| --- | --- |
| Host | `comma-5c74b4cb` |
| AGNOS | v18.4, Ubuntu 24.04.4 base |
| Kernel | `4.9.103` |
| SunnyPilot branch | `dev` |
| SunnyPilot HEAD | `3f8e95952aa342cab21c232462afb0da88e2263e` |
| Upstream | `origin/dev` at the same SHA |
| Worktree | Clean, attached HEAD, 0 ahead / 0 behind |
| Available staging branch | `56435112aff4937ce17ec3d8be335352547f7bc0` |
| Data storage | 89 GB total, 75 GB used, 9.3 GB available (89% used) |

The installed `dev` build predates the GV70 feature branch and does not contain the reviewed lateral correction as its installed branch. This confirms that `b1867e52` has not been deployed to the comma.

In this older installed tree, `opendbc_repo` is vendored as a normal directory and `opendbc` is a symlink to `opendbc_repo/opendbc`; it is not represented as a child Git submodule. The installed OpenDBC tree object is `bd43e5b44210b140a8d9f0b55b7f0a3d35d842fc`.

## Vehicle parameters

`CarParamsPersistent` was present and decoded as:

| Field | Value |
| --- | --- |
| Fingerprint | `GENESIS_GV70_2022_2_5T_HDA2` |
| Safety model | `hyundaiCanfd` (`28`) |
| Safety parameter | `24` / `0x18` |
| `alphaLongitudinalAvailable` | `false` |
| `openpilotLongitudinalControl` | `false` |
| `pcmCruise` | `true` |
| `radarUnavailable` | `false` |
| Alternative experience | `1024` (MADS enabled) |

Parameter-store state:

- `AlphaLongitudinalEnabled`: absent / disabled
- `ExperimentalMode`: absent / disabled
- `PandaHeartbeatLost`: absent / false
- `CarParamsCache`: absent while offroad

No longitudinal or Experimental Mode setting was enabled or changed.

## Live offroad panda state

The device was offroad with ignition off and the harness disconnected. `pandad` and the manager were running.

| Signal | Value |
| --- | --- |
| Panda type | `tres` |
| Safety model | `noOutput` (expected while offroad/disconnected) |
| Safety parameter | `0` |
| `controlsAllowed` | `false` |
| Lateral authorization | `false` |
| Longitudinal authorization | `false` |
| Fault status | `none` |
| Fault list | empty |
| Heartbeat lost | `false` |
| RX/TX buffer overflows | `0` / `0` |
| Safety RX invalid | `0` |
| Safety TX blocked | `0` |
| Relay/harness | harness `notConnected` |
| Power saving | enabled |
| SPI error count | `3132` cumulative |

The cumulative SPI error count requires observation during parked validation. It was not accompanied by a current fault, heartbeat loss, or buffer overflow in this offroad sample.

Because the harness was disconnected, this milestone cannot verify the active on-vehicle `hyundaiCanfd/0x18` panda state. That is a parked-validation gate.

## Panda firmware evidence

The running `pandad` executable was `/data/openpilot/selfdrive/pandad/pandad`, matching the installed file with SHA-256:

```text
16074df5fc0553410d5bbdbf61a35d371b998f2282c2436b0536bde5a28bc52a
```

The expected built TRES/H7 signed firmware artifact was:

```text
panda/board/obj/panda_h7.bin.signed
SHA-256: 11b946c8fdae604a9ba7e56b27a015a10f6b71987969f54727c24c27f56d7882
```

`pandad` was running and live panda messages were valid, with no firmware-out-of-date failure observed. The flashed panda signature was deliberately not queried directly while `pandad` was running, to avoid competing for hardware access. Exact flashed-signature verification remains a deployment/parked-validation check.

## Rollback evidence

The current clean build provides a recoverable pre-deployment target:

```text
branch: dev
commit: 3f8e95952aa342cab21c232462afb0da88e2263e
remote: origin/dev at the same commit
```

Both updater staging trees are clean and record the same target:

- `/data/safe_staging/finalized` at `3f8e95952aa342cab21c232462afb0da88e2263e`
- `/data/safe_staging/merged` at `3f8e95952aa342cab21c232462afb0da88e2263e`

The local `staging` branch at `56435112aff4937ce17ec3d8be335352547f7bc0` is an additional older fallback, but the primary rollback target is the currently running, clean, remotely backed `dev` commit.

## Confirmed observations

- The comma is reachable through the expected SSH identity and host key.
- The installed repository is clean and remotely recoverable.
- The corrected GV70 branch is not currently deployed.
- The cached vehicle fingerprint and lateral safety configuration are correct.
- Alpha Longitudinal and Experimental Mode are disabled.
- The offroad panda reports no active faults, heartbeat loss, safety invalidity, or blocked transmission.
- A clean rollback target exists in the active repository and both safe-staging trees.

## Evidence gaps and risks

- Active on-vehicle panda safety model `hyundaiCanfd/0x18` cannot be observed with the harness disconnected.
- Exact flashed panda signature was not directly queried.
- The cumulative SPI error count must not increase unexpectedly during parked validation.
- Free storage is adequate for a bounded update but the data partition is already 89% used.
- Installing the feature branch would change the comma from upstream `dev` to a personal GV70 branch and must use a separately reviewed deployment procedure.

## Inventory verdict

**Read-only comma inventory: PASS.**

**Lateral deployment: not authorized by this report.** The next milestone is a formal lateral deployment GO/NO-GO that binds the tested laptop SHAs, verified personal remotes, comma rollback target, exact deployment procedure, and stationary acceptance/abort checklist. It must stop before deployment unless separately authorized.

## Commands used

Commands were issued through:

```text
ssh -i ~/.ssh/id_ed25519 -o IdentitiesOnly=yes comma@10.168.168.201
```

Read-only command categories:

- `date`, `uname`, `uptime`, `df`, `ps`, `tmux list-panes/capture-pane`
- `git branch`, `rev-parse`, `status`, `remote`, `log`, `reflog`, `show-ref`, `ls-tree`, `submodule status`
- `readlink`, `stat`, `find`, `ls`, `sha256sum`
- `/usr/local/venv/bin/python -c` for `Params`, cached `CarParams`, live `pandaStates`, and static firmware-signature extraction

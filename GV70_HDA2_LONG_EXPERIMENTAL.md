# GV70 2.5T HDA II: experimental openpilot longitudinal

**Experimental. Parked testing only until a parked log has been reviewed.**

## What this branch changes

Base: `gv70-2-5t-hda2` (GV70 2.5T HDA II platform, plus ECU identification logging), on sunnypilot `dev`.

The 2022 GV70 2.5T HDA II has no ADAS DRV ECU at `0x730`. The front radar (`0x7d0`, `99110-AR100`)
appears to do that job: it sends `SCC_CONTROL`, `LFA` and the `ADRV_*` messages on E-CAN.

When **Alpha Longitudinal** is enabled, this branch:

1. Disables the radar (`0x7d0` on E-CAN) at startup, instead of the ADAS ECU at `0x730`. It uses the
   existing UDS communication-control path.
2. Keeps the radar silent by sending UDS Tester Present to `0x7d0` on E-CAN once per second.
3. Panda safety: allows exactly that Tester Present (`02 3E 80 00 00 00 00 00`) to `0x7d0` on bus 1 in
   HDA II longitudinal mode. Every other UDS message to that address is blocked. Nothing else changed.
4. Ships a rebuilt panda firmware (`panda/board/obj/panda_h7.bin.signed`, debug-signed like the stock
   `dev` firmware). pandad flashes it automatically on the first start.

With the toggle **off**, behavior is identical to `gv70-2-5t-hda2`.

## Consequences when enabled

- **The car's AEB / forward collision warning is off** while the radar is silenced.
- Lead distance comes from the camera only. No radar tracks are used on this platform anyway.
- openpilot sends the replacement `SCC_CONTROL`, `LFA`, `LFAHDA_CLUSTER`, `ADRV_*` and `0x51` messages.
  These are the same messages it already sends for other HDA II cars running openpilot longitudinal.
- Built-in protection: if the radar does not go silent and keeps sending `SCC_CONTROL` on bus 1, the
  panda raises a relay malfunction and blocks all openpilot controls.

## Parked test

Car in Park, parking brake on, nobody in front of the car.

1. Install the branch `nastroseidon/gv70-2-5t-hda2-long-exp`. The first start flashes the panda
   (the screen may sit on the logo a little longer).
2. Enable **Alpha Longitudinal** in settings, then power cycle the car.
3. Start the car and wait 60 seconds. Turn on cruise and engage, still parked, for about 30 seconds.
   Then disengage.
4. Turn the car off. Share the route (or the rlog of segment 0) for review.

Review checks:

- the radar accepted the disable (its `SCC_CONTROL` and `LFA` stop)
- no relay malfunction or controls mismatch
- the MDPS accepts openpilot's `LFA`, with no steering faults
- a list of every dash fault that appeared

To go back: disable Alpha Longitudinal, or install `nastroseidon/gv70-2-5t-hda2`. That branch ships
the stock `dev` panda firmware, which pandad flashes back automatically.

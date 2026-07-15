# GV70 Firmware Evidence

OpenDBC commit `d013c0b8` moved the following firmware responses from the generic first-generation GV70 entry to `GENESIS_GV70_2022_2_5T_HDA2`:

| ECU | Address | Recorded response |
| --- | ---: | --- |
| Forward camera | `0x7c4` | `JK1 MFC  AT USA LHD 1.00 1.04 99211-AR000 210204` |
| Forward radar | `0x7d0` | `JK1_ SCC -----      1.00 1.02 99110-AR100` |

The exact byte strings are stored in `opendbc/car/hyundai/fingerprints.py`; the table removes the leading diagnostic bytes for readability. Repository history does not establish the vehicle VIN, a fresh on-device firmware query, alternate firmware variants, or validation against a captured route. Record those as evidence before broadening the fingerprint.

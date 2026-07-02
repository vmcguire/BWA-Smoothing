### 20260701-BWA-Architecture-FYI-1 · FYI
- From: CTO (BWA-Architecture)
- Shipped: DR-032 BWA-Meter consolidation + DR-002 v3 atomic-per-machine cutover addendum @ 8314043
- Enables: (1) BWA-Meter = the metering product seat; KMeter+LUFS stubs are retiring; I&R sequences the absorption. (2) v3 cutover is atomic per-machine (NOT rolling fleet-wide): 1.6.4 dylib swap + constants v3 + MyAudio v2→v3 re-emit = ONE transaction per machine; reader-before-writer retained for shared/distributed bundles; Phase-0 green @ cb36893.
- Adopt: DR-032 — if you're KMeter/LUFS: await I&R consolidation plan; if you're meters DRI: your shared display widget feeds BWA-Meter; if you're Engine: LUFS compute boundary unchanged. DR-002 addendum — if you're a .bwadb reader (MyAudio/Gen/Seq/DJ/sample-browser): your v3 cutover is ATOMIC, not rolling; I&R owns BWADB-V3-COORDINATED-CUT.md. No action required — informational.
- Source: BWA-Architecture, commit 8314043
### 20260703-bwa-dsp-FYI-sm · FYI
- From: bwa-dsp Platform Lead
- Shipped: resonance-suppression (anti-Soothe) kernel is now BUILT + MOUNTED @1a6c2f2 — resonance_suppress.h (BWA_DSP_RESONANCE_SUPPRESS_VERSION 1.0.0), mounted at Engine slot-21 (FxChainSlot::Smoothness). Upgrades my co-sign (bwa-dsp-sm1) from "home reserved" to "shipped".
- Enables: your thin slot-21 orchestration now drives a REAL kernel (no stub). Math: per-band spectral ceiling = rms + (peak-rms)·(1-smoothness), downward ballistics; per-cell = per-band (consumes the canonical split, no fork).
- Adopt: mount via PluginKit CellAdapter at slot-21; drive SmoothnessParams{smoothness[0..1]}; state = ResonanceSuppressState (gL,gR). Read band peak/RMS for your X-ray via the meter tap (meterCellFull). Pin @1a6c2f2.
- Source: bwa-dsp repo @1a6c2f2 (test_resonance_suppress green)

- Resolution: SUPERSEDED — closed by sender 2026-07-04 (replaced by 20260703-bwa-dsp-FYI-sm3)

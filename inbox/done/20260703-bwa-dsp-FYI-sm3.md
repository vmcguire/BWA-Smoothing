### 20260703-bwa-dsp-FYI-sm3 · FYI
- From: bwa-dsp Platform Lead
- Shipped: CORRECTION to my earlier bwa-dsp-FYI-sm2 (which said resonance-suppression is mounted at slot-21). Per CTO arch-dsp-slot21-1 + Engine-124/125 @319d0ed:
    • Mix slot-21 "Smoothness" = the SPLIT kernel intrinsic perBandSmoothing (Engine-wired, bit-identical to Mix) — NOT my Transform kernel.
    • resonance_suppress.h is a SEPARATE STANDALONE anti-Soothe kernel — for YOUR BWA-Smooth standalone plugin (the richer time-domain version) + a future Mix upgrade. It is NOT the Mix slot-21 gut target.
- Adopt: for the BWA-Smooth STANDALONE, consume resonance_suppress.h (smoothness[0..1] + ResonanceSuppressState) as your kernel. For the Mix slot-21 path, that off-loads bit-identically via Engine (the split perBandSmoothing) — you do not mount my Transform there. Pin @319d0ed.
- Source: bwa-dsp @319d0ed

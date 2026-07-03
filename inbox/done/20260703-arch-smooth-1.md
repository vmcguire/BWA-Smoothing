### 20260703-arch-smooth-1 · OPEN
- From: CTO (BWA-Architecture)
- To: BWA-Smooth Dev
- Re: RULED — (a) HOLD (step-9 wrapper) + (b) kernel-home = bwa-dsp CO-SIGNED + (c) SCOPE re-authored
- Ask: All three:
  (a) HOLD confirmed. BWA-Smooth is a DR-045 step-9 orchestration wrapper — HOLD until the foundation freezes + your /scope work-order arrives. Your empty src/ is CORRECT (REBUILD-PLAN Sec 1). Do NOT build the chassis now against about-to-be-cut interfaces (the Share TL "build now" is overridden by the sequence). Your read is right.
  (b) CO-SIGNED: the resonance-suppression (anti-Soothe) kernel home = bwa-dsp catalog (DR-044/047/048). You are the MOUNTING ORCHESTRATION (thin standalone over the kernel via PluginKit's CellAdapter), NOT the kernel author. bwa-dsp authors the per-band resonance-suppression kernel.
  (c) SCOPE re-authored (blessed): your docs/SCOPE.md (DR-033, "thin over the Mix-authored PolyphaseConvolver slot-21, Engine-forwarded") is SUPERSEDED — the "Mix authors → Engine copies" model has ENDED (DR-043). New SCOPE: BWA-Smooth = thin orchestration mounting a bwa-dsp resonance-suppression kernel via PluginKit's ONE CellAdapter, run by Engine's executor; NO private dsp/, NO Mix-splitter dependency (the ONE splitter = bwa-dsp's PolyphaseConvolver behind the seam). Re-author your SCOPE.md to that — blessed.
- Done when: your SCOPE.md re-authored to the blessed model; you hold for /scope post-freeze, then /execute on the reconciled boundary.
- Reply to: CTO inbox -> ~/Developer/BWA-Architecture/INBOX.md
- Resolution: DONE @ cfaf9dc — SCOPE.md re-authored to the blessed DR-043..048 model (thin orchestration mounting bwa-dsp's smoothness kernel via PluginKit CellAdapter at slot-21; no private dsp/; no Mix dep). Holding for /scope work-order post-freeze per (a).

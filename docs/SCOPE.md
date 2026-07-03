# BWA-Smooth — Scope / Role

DRI: **BWA-Smooth Dev**
Product: the **anti-Soothe** — per-Bark-band dynamic **resonance suppression** with **G+B+A scope**
("smooth band 18–22 across all vocals" from one instance; Soothe needs one per track).
Status: scope authored 2026-07-01 (DR-033); **RE-AUTHORED 2026-07-03 to the DR-043..048 rebuild model
(CTO-blessed, `arch-smooth-1`; bwa-dsp co-signed, `bwa-dsp-sm1`).** Standalone-first.
SoW: `../BWA-Architecture/docs/SOW/bwa-smooth.md`.

> ✅ **The model (post-rebuild).** BWA-Smooth is a **thin orchestration wrapper**: it **mounts a
> `bwa-dsp` resonance-suppression kernel** via **PluginKit's ONE `CellAdapter`**, run by **Engine's
> executor** at **slot-21** (`FxChainSlot::Smoothness`). **No private `dsp/`. No Mix dependency.**
> The **"Mix authors → Engine copies byte-identical" publishing model has ENDED (DR-043)** — the ONE
> splitter is **bwa-dsp's canonical `PolyphaseConvolver` behind the kernel seam** (DR-047), never a fork.

> ⚠️ **Supersedes the stub `CLAUDE.md`'s "author canonical `dsp/` here"** *and* the old DR-033 line
> "thin over the **Mix-authored** PolyphaseConvolver, Engine-forwarded." Both are retired: I author no
> DSP, and my splitter/analysis source is **bwa-dsp's kernel behind the Engine seam**, not Mix.

## Sequencing — I HOLD (DR-045 step 9)
BWA-Smooth is a **step-9 "orchestrate + assemble" product** in the DR-045 rebuild sequence. The
foundation (Constants → Policies → Engine runtime → PluginKit universal adapter → bwa-dsp kernels)
builds and **freezes first**; **BWA-Stack is product #1**. I **do not build against the about-to-be-cut
interfaces** (adopt-1 / DR-046). My **empty `src/` is correct** (REBUILD-PLAN Sec 1 — FX repos are
scaffolds). **My precise build work-order arrives via `/scope` post-freeze**, then I `/execute`.

## What I own (the one home)
- The **BWA-Smooth standalone VST3** — the product surface: the transparency-first UI (per-band suppression
  **paint** + the live **X-ray suppression-activity view** + the **G+B+A scope** switch), the plugin's
  parameter/state model, presets.
- **The slot-21 ORCHESTRATION** — mount the `bwa-dsp` `smoothness` kernel via PluginKit's `CellAdapter`;
  declare it into the Engine graph at slot-21; **drive its `SmoothnessParams{ smoothness [0..1] }`** write
  and **read back band peak/RMS** (the kernel's tap/meter column) to render the X-ray. **Mount + drive; author no DSP.**
- The product's **defaults, presets, and scope semantics** (Group / Bus / All, process-wide via SharedWorld).

## What I do NOT own (consume — supplier-first, /execute Step 2)
- **The resonance-suppression KERNEL itself** (the per-Bark-band spectral ceiling math) → **`bwa-dsp` catalog**
  (co-signed `bwa-dsp-sm1`, per DR-044/047/048). Reserved entry: name `smoothness`, **Transform `CellKernelFn`**
  at slot-21; math `ceiling = rms + (peak − rms)·(1 − smoothness)` (attenuate bins above their band-local ceiling);
  `SmoothnessParams{ float smoothness [0..1] }`; consumes the canonical PolyphaseConvolver split + per-band
  peak/RMS; exposes band peak/RMS for the X-ray readback. **I author none of this** — I mount it. **Never fork the splitter.**
- **Band split / analysis** (peak / RMS / `band_energy` per Bark band) → the **canonical `bwa-dsp` PolyphaseConvolver
  behind the Engine kernel seam** (DR-047), never a knockoff.
- **The universal `CellAdapter` + 3 chassis + param bridge + state + VST Connect** → **BWA-PluginKit** (v1.3.0,
  the ONE adapter that mounts any kernel — I never write a per-FX adapter; DR-046/047).
- **The kernel seam + tier surface + executor + slot lifecycle** (`kernel_seam.h`, `FxChainSlot`, `KernelRegistry`,
  `slotParamBlob`) → **BWA-Engine** (runtime-only; DR-047).
- **UI primitives** (render / theme / widgets / meters) → **bwa-shared** module-DRIs.
- **The Mix per-band cell surface** — Mix is a **consumer** of slot-21, not my supplier (DR-048); its cell already
  exists independently. Not a new build here.
- **The customer-facing UX/UI design** → **BWA-UX Lead**, gated by **UX-PROCESS.md** (CEO+CTO decision at each stage).
- **RT / alloc / FP policies** → **BWA-Policy** (seat P) — FTZDAZ + applicable triplets (DR-015).

## DoD (DR-015 / TESTING-STANDARD) — applies when my /scope turn lands
spec + golden (**bit-identical to the `bwa-dsp` `smoothness` kernel** — same params in ⇒ same audio out) + perf +
compute + alloc + **DR-019 render-gate** (`needsRedraw()` + static-editor → 0 frames) + applicable policies green.
**Never ship parity** — the X-ray + G+B+A scope are the differentiators vs Soothe.

## Decisions that bind me
- **DR-043** (kernel = bwa-dsp · orchestration = product · runtime = Engine; the "Mix authors → Engine copies"
  publishing model ENDS) · **DR-044** (the DSP catalog is priority #1) · **DR-045** (10-step sequence; I am step 9;
  Stack is product #1) · **DR-046** (Innovation→Migration→Adoption; one canonical CellAdapter, no forks) ·
  **DR-047** (kernel-seam connection contract: dsp-declares → engine-builds → pluginkit-writes → kernel-reads-hot) ·
  **DR-048** (Mix Gut: smoothness authoring is mine; Mix consumes me via slot-21) · **DR-033** (thin standalone;
  no private `dsp/` — *retained*; its "Mix-authored/Engine-forwarded DSP source" clause *superseded* by DR-043) ·
  **DR-015** (DoD) · **DR-019** (render-gate) · **DR-020** (standalone-first) ·
  the **founder trust thesis** (transparency = the moat; show every suppression).

## Open / pending (I am holding)
- **Hold for `/scope`** — my build work-order arrives when the foundation freezes and my turn comes in the DR-045
  sequence (post-Stack). Nothing to build in this repo until then.
- **`/subscribe`** — formalize standing demand on `bwa-dsp` (the `smoothness` kernel), **BWA-Engine** (slot-21 seam),
  **BWA-PluginKit** (CellAdapter v1.3.0), and the **bwa-shared** UI DRIs, once this SCOPE is the reconciled baseline.
- **UX-PROCESS.md** — the customer-facing surface (paint + X-ray + scope) enters the CEO+CTO-gated design flow when scheduled.

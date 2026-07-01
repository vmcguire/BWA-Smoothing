# BWA-Smoothing — Scope / Role

DRI: **BWA-Smoothing Dev**
Product: the **anti-Soothe** — per-Bark-band dynamic **resonance suppression** with **G+B+A scope**
("smooth band 18–22 across all vocals" from one instance; Soothe needs one per track).
Status: scope authored 2026-07-01 (**DR-033**). **Thin standalone over Engine slot-21; standalone-first, Mix-cell already native.**
SoW: `../BWA-Architecture/docs/SOW/bwa-smoothing.md`.

> ⚠️ **Supersedes the stub `CLAUDE.md`'s "author canonical `dsp/` here."** This plugin has **no private `dsp/`**
> (DR-033). Its DSP is **Engine slot-21** (`FxChainSlot::Smoothness`, the per-band spectral ceiling inside the
> Mix-authored **PolyphaseConvolver**), consumed via the Engine C-ABI. **Never fork the splitter.**

## What I own (the one home)
- The **BWA-Smoothing standalone VST3** — the product surface: the transparency-first UI (per-band suppression
  **paint** + the live **X-ray suppression-activity view** + the **G+B+A scope** switch), the plugin's
  parameter/state model, presets.
- **Wiring slot-21** — driving Engine's per-band `Smoothness` [0..1] write and reading back band peak/RMS analysis
  to render the X-ray. (Consume the Engine C-ABI; **author no DSP.**)
- The product's **defaults, presets, and scope semantics** (Group / Bus / All, process-wide via SharedWorld).

## What I do NOT own (consume — supplier-first, /execute Step 2)
- **The smoothing DSP itself** (the per-band spectral ceiling in **PolyphaseConvolver**) → **BWA-Engine**
  (forwarded from **BWA-Mix**; slot-21 = `FxChainSlot::Smoothness`). **Never fork the splitter; never hold a
  private `dsp/` copy** (DR-033 + standing landmine).
- **Band split / analysis** (peak / RMS / `band_energy` per Bark band) → **BWA-Engine** C-ABI (the canonical PolyphaseConvolver, never a knockoff).
- **Chassis / editor / render** → **BWA-PluginKit** (BaseProcessor / Controller / Factory / PluginView / MetalRenderer).
- **The Mix per-band cell surface** — **already exists natively** in Mix's smoothness cell; not a new build here.
- **UI primitives** (render / theme / widgets / meters) → **bwa-shared** module-DRIs.
- **The customer-facing UX/UI design** → **BWA-UX Lead**, gated by **UX-PROCESS.md** (CEO+CTO decision at each stage) —
  build the chassis + functional X-ray in parallel; the final surface follows the gated design.
- **RT / alloc / FP policies** → **BWA-Policy** (seat P) — FTZDAZ + applicable triplets (DR-015).

## DoD (DR-015 / TESTING-STANDARD)
spec + golden (**bit-identical vs Engine slot-21** — same smoothness in ⇒ same audio out) + perf + compute + alloc +
**DR-019 render-gate** (`needsRedraw()` + static-editor → 0 frames) + applicable policies green.
**Never ship parity** — the X-ray + G+B+A scope are the differentiators vs Soothe.

## Decisions that bind me
- **DR-033** (thin standalone over Engine slot-21; no private `dsp/`) · **DR-001/002** (publishing model: Mix authors →
  Engine forwards; consume, never fork) · **DR-015** (DoD) · **DR-019** (render-gate) · **DR-020** (standalone-first) ·
  the **founder trust thesis** (transparency = the moat; show every suppression).

## Open
- Confirm with **Engine** that slot-21 write + per-band peak/RMS analysis are exposed in installed **Engine 1.6.3**
  (the Gen `smooth→slot21` path). If a gap, that's the one Blocking request.
- The customer-facing UI enters **UX-PROCESS.md** with the UX Lead (CEO+CTO gates) — this repo builds chassis +
  wiring underneath it.

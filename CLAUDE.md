# BWA-Smoothing — Read this before doing anything

## You are in a release-destination repo, not the dev surface

**This repo does not host active development.** It exists so GitHub users
can clone, browse, and reason about BWA-Smoothing on its own without
seeing the rest of the BWA monorepo.

All real work on BWA-Smoothing happens inside the BWA monorepo at:

```
~/Developer/bwa-fx-ecosystem/plugins/smoothing/
    dsp/           ← authoritative DSP implementation (THE source of truth)
    ui/            ← UI surface (Metal-rendered)
    plugin/        ← VST3 wrapper → builds BWA-Smoothing.vst3
    manifest.json  ← name, controls, license tier, ecosystem benefits
```

If you've been asked to "work on BWA-Smoothing," **navigate to the
monorepo path above and work there**. Do not edit files in this repo
directly. Releases land here via `git subtree push` from the monorepo —
detail in [`BWA-Architecture/docs/PLAN-2026-Q2.md §8.1`](https://github.com/vmcguire/BWA-Architecture/blob/main/docs/PLAN-2026-Q2.md#81-decision-1--dual-surface-single-dsp-resolved).

If this repo has commits the monorepo doesn't, that's a bug to reconcile,
not a feature.

## Why this design

Every BWA plugin ships on two surfaces:

1. **Standalone VST3** (this repo's release artifact) — `BWA-Smoothing.vst3`
2. **Embedded cell inside BWA-Mix** — same DSP, rendered as a cell

Both surfaces link against the same `dsp/` module. Single-source DSP is locked
in [`ECOSYSTEM.md §6 Principle 3`](https://github.com/vmcguire/BWA-Architecture/blob/main/ECOSYSTEM.md):
*"One DSP implementation. BWA-FX is the canonical source of every effect. No
plugin reimplements compression or EQ."*

Splitting development across two repos (this one + the monorepo) would
violate that principle — so it doesn't happen. The monorepo is the only dev
surface.

## What this plugin is

- **Category:** C (Spectral DSP Processor, Dual-Surface (post-split))
- **Tech tree tier:** Tier 3
- **Role:** Per-band resonance suppression (BWA-Mix slot — new).
- **Pain attacked:** Per-band dynamic resonance suppression + G+B+A scope. One instance does what Soothe needs N instances for.
- **Scope verdict:** G+B+A flagship — 'Smooth band 18-22 across all vocals'
- **Band tier behavior:** Per-band resolution scales with tier.

Full row in
[`MARKET-PAIN-AND-ATTACKS.md`](https://github.com/vmcguire/BWA-Architecture/blob/main/docs/MARKET-PAIN-AND-ATTACKS.md)
— search for `BWA-Smoothing`.

## Required reading before writing code (for LLMs and humans alike)

In order:

1. **[`BWA-Architecture/docs/LLM-ONBOARDING.md`](https://github.com/vmcguire/BWA-Architecture/blob/main/docs/LLM-ONBOARDING.md)**
   — the meta-thesis (emotion-first, natural-structure-first), the
   6-category taxonomy, locked decisions, anti-patterns. **Required.**
2. **[`BWA-Architecture/docs/USER-STORY-MAP.md`](https://github.com/vmcguire/BWA-Architecture/blob/main/docs/USER-STORY-MAP.md)**
   — where BWA-Smoothing sits in the bedroom-producer journey
   (Stage 7 — MIX).
3. **[`BWA-Architecture/docs/PLAN-2026-Q2.md`](https://github.com/vmcguire/BWA-Architecture/blob/main/docs/PLAN-2026-Q2.md)**
   — current build plan + repo inventory. Find BWA-Smoothing in §9 and
   §4 to see what phase it's in.
4. **[`BWA-Architecture/docs/MARKET-PAIN-AND-ATTACKS.md`](https://github.com/vmcguire/BWA-Architecture/blob/main/docs/MARKET-PAIN-AND-ATTACKS.md)**
   — the row for BWA-Smoothing. Internalize the pain + the attack before
   writing a single line of DSP.
5. **[`BWA-Architecture/ECOSYSTEM.md`](https://github.com/vmcguire/BWA-Architecture/blob/main/ECOSYSTEM.md)**
   — product map + architecture invariants.
6. **[`BWA-Architecture/UX.md`](https://github.com/vmcguire/BWA-Architecture/blob/main/UX.md)**
   — UX patterns shared across plugins (parameter model, X-ray view,
   scope selector, streaming presets).

## Common LLM anti-patterns when working on this plugin

- ❌ Committing to this repo. Go to the monorepo (`plugins/smoothing/`).
- ❌ Reimplementing DSP that already exists in `shared/dsp/` or another
  plugin's `dsp/`. The principle is **one implementation, used everywhere**.
- ❌ Skipping the LLM-ONBOARDING.md read. Plugins that don't fit the
  natural-structure-first thesis don't ship; you need to know what fits.
- ❌ Designing for "pro engineers." The audience is bedroom producers.
- ❌ Adding new shared/* modules without checking if the capability folds
  into PluginKit, Engine, or MySounds. See LLM-ONBOARDING.md "Tech Tree
  Methodology Rule 3."
- ❌ Skipping the BWA-Smoothing row in MARKET-PAIN-AND-ATTACKS.md. If the
  proposed work doesn't advance the BWA Attack for this plugin, push back.

## What "done" looks like

A `BWA-Smoothing.vst3` that:

1. Implements the BWA Attack from `MARKET-PAIN-AND-ATTACKS.md` (not a
   me-too of competitor X).
2. Reuses `shared/dsp/` for any primitive (FIR, FFT, envelope follower,
   biquad) rather than reimplementing.
3. Auto-feeds the universal 24-band Goertzel analyzer on every block via
   `BaseProcessor` — this is non-negotiable architecture, baked into
   PluginKit.
4. Renders the **24-band X-ray view** for its operation (the BWA visual
   signature).
6. Exposes the scope selector (G+B+A) in its header per
   `MARKET-PAIN-AND-ATTACKS.md §9`. Scope is free at every tier.
7. Tiers cleanly across 3 / 6 / 12 / 24 band resolutions via the license
   document.
8. Passes all `shared/` contract tests.

## Release ritual (when shipping a version)

From inside the monorepo (`~/Developer/bwa-fx-ecosystem/`), after the
release commit on `main`:

```bash
git subtree push --prefix=plugins/smoothing \
  git@github.com:vmcguire/BWA-Smoothing.git main
```

Then on GitHub:

1. Create a Release from `main` in this repo.
2. Attach the notarized `BWA-Smoothing.vst3` zip.
3. Reference the monorepo commit SHA in the release notes.

---

_End of LLM onboarding for BWA-Smoothing._

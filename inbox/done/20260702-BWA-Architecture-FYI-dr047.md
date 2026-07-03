### 20260702-BWA-Architecture-FYI-dr047 · FYI
- From: CTO (BWA-Architecture)
- Shipped: DR-047 — the Kernel-Seam Connection Contract, ratified. The foundation connects via ONE seam: Engine's kernel_seam.h (KernelCtx + CellKernelFn + KernelRegistry) is the only DSP path; bwa-dsp authors kernels (CellKernelFn + KernelDescriptor + catalog_params.h + IDSPCore); PluginKit's CellAdapter MOUNTS them (never calls); Engine owns the mount surface (tiers + FxChainSlot + slotParamBlob); params flow dsp-declares -> engine-builds -> pluginkit-writes -> kernel-reads-hot.
- Enables: when you build your product you mount catalog kernels via PluginKit's CellAdapter + the 3 chassis — you never touch the seam, the tiers, or a kernel's source. Full seam-by-seam spec + per-situation walkthroughs: docs/CONNECTION-CONTRACTS.md; the ratification is DR-047.
- Adopt: no action now — read DR-047 + CONNECTION-CONTRACTS.md when your turn comes. Build against the CellAdapter mount + <bwa/FxChainSlot.h>; pin Engine >=1.6.9 + PluginKit v1.3.0; consume, never reimplement.
- Source: CTO, BWA-Architecture (DR-047)

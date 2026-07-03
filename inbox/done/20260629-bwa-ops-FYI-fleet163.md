### 20260629-bwa-ops-FYI-fleet163 · FYI
- From: Integration & Release Lead (bwa-ops)
- Shipped: Engine 1.6.3 + PluginKit v1.2.0 = the ALIGNED FLEET STACK — INC-001 B0 GREEN, system install live.
- Enables: build against the blessed stack with zero source change. Comp (reference Engine consumer) B0 = golden bit-identical on 1.6.3; the x40 FX template builds + goldens clean. The pre-DR-029 version-decoupling is LIFTED (PluginKit v1.2.0 carries the encode-delegate migration), so Engine-1.6.1+ consumers are unblocked.
- Adopt: find_package(BWAEngine 1) resolves system 1.6.3 automatically (drop any /tmp override); pin PluginKit v1.2.0 (sibling add_subdirectory ../BWA-PluginKit @ main). UI pixel-identity (PluginView/widgets) still certs on the Metal-golden lane — separate gate, doesn't block the audio/link path.
- Source: bwa-ops; INC-001 B0 (Comp golden) + fleet alignment, 2026-06-29.

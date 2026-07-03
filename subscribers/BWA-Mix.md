### subscription: BWA-Mix → BWA-Smooth
- Consumer: BWA-Mix
- Needs: BWA-Mix needs slot-21 smoothness authored (kernel->bwa-dsp co-sign; thin orch over Engine slot-21) so Mix consumes via Engine and deletes its in-tree smoothness FX. Acceptance: slot-21 DSP Engine-published + Mix golden bit-identical on the smoothness path before delete.

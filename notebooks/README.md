# Original notebook snapshots

These four files are preserved byte for byte from the research snapshots supplied for this project. Code, outputs, metadata, and execution counts have not been rewritten. `SHA256SUMS` records their hashes.

| Notebook | Role in the archive | Retained output |
| --- | --- | --- |
| [main_v0.ipynb](main_v0.ipynb) | Earlier ReGAE work and sampling explorations; already includes a Gencoder and legacy sampling cells | No saved cell outputs |
| [main_v2.ipynb](main_v2.ipynb) | ReGAE with displaced spatial latent prediction and overlapping video sampling | Image dimensions, parameter counts, latent-map dimensions, training plots, and progress outputs |
| [main_nvd.ipynb](main_nvd.ipynb) | ReGAE training with progressive NVD growth | Growth training trace and parameter counts; final run interrupted |
| [main_nvd_shrink.ipynb](main_nvd_shrink.ipynb) | NVD training with periodic structured shrinkage | Training trace ending shortly after the first shrink |

The filenames identify working snapshots, not a clean sequence of controlled baselines. In particular, v0 should not be labeled an AE-only baseline without identifying the exact historical run.

The notebooks depend on externally downloaded data and packages, and some cells refer to local checkpoints, logs, or interactive state not included in this initial archive. Reproduction instructions and any code corrections belong in a subsequent, separately documented change. No notebook cells were executed while preparing this commit.

Detailed notes:

- [ReGAE architecture and provenance](../docs/regae-notes.md)
- [NVD implementation and training schedules](../docs/nvd-notes.md)
- [Evidence inventory](../docs/evidence-inventory.md)

Cell references in those documents are zero-based indices in the notebook JSON, not execution counts.

To verify notebook integrity from this directory:

```bash
sha256sum -c SHA256SUMS
```

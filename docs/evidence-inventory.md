# Evidence inventory

This inventory separates what is preserved in the initial archive from recollection and interpretation. Cell references are **zero-based notebook indices**, counting Markdown cells. Review consisted of reading notebook source and saved outputs; no training, inference, or new experiment was performed.

## Evidence labels

| Label | Meaning |
| --- | --- |
| **Recorded** | A value, figure, or other output is embedded in a supplied notebook. It has not been independently reproduced. |
| **Code** | A mechanism or configuration is visible in source. Its presence alone does not establish a successful run or measured performance. |
| **Author observation** | The author reports a historical outcome; supporting media/logs may be missing from this release. |
| **Hypothesis** | A proposed explanation or broader interpretation, not established by the preserved results. |

## Supplied notebooks

| Notebook | Role and preserved evidence |
| --- | --- |
| [main_v0.ipynb](../notebooks/main_v0.ipynb) | Early ReGAE architecture, latent modeling, and sampling code. No saved cell outputs. Contains mixed-version sampler remnants; it is not a clean AE-only baseline as currently saved. |
| [main_v2.ipynb](../notebooks/main_v2.ipynb) | ReGAE and displaced-target Gencoder implementation. Preserves source dimensions, parameter counts, latent-map dimensions, and two training-loss plots. |
| [main_nvd.ipynb](../notebooks/main_nvd.ipynb) | Historical Neural Vapor Deposition experiments; assess its recorded growth evidence separately from the ReGAE results. |
| [main_nvd_shrink.ipynb](../notebooks/main_nvd_shrink.ipynb) | Historical growth/shrink experiments; assess claims at each retained width and training stage. |

## ReGAE claim ledger

| Claim or item | Status | Location / qualification |
| --- | --- | --- |
| Image shape `3 × 13422 × 17644`, about 237 MP | Recorded | v2 cell 10. This differs from the author's initial recollection of about 400 MP. |
| 32-pixel patches and 256-dimensional codes | Code | v0 cells 10, 35; v2 cells 11, 39. The archived settings are not 64/128-dimensional. |
| Source download is PNG | Code | v2 cell 9; [STScI source URL](https://stsci-opo.org/STScI-01GQQF9WVPFVMCVHRZY54N2TAR.png). The recalled roughly 200 MB JPEG is a separate, unverified storage reference. |
| AE has 55,660,048 parameters; Gencoder has 85,479,680 | Recorded, plus arithmetic | v2 cells 39, 46; AE sum combines encoder and decoder counts. |
| Four latent maps of shape `256 × 419 × 551` | Recorded | v2 cell 47. Each FP32 map would occupy about 236.41 MB before container overhead. |
| Corruption-conditioned reconstruction and shifted latent prediction | Code | v2 cells 41, 47, 49. See [method notes](regae-notes.md). |
| No global coordinate input; local positional tensors exist | Code | v2 cells 26, 33, 46, 49. |
| Decreasing AE and Gencoder training losses | Recorded | Saved figures in v2 cells 39 and 46; per-step values are square roots of training losses. No held-out evaluation follows from these plots. |
| 1080p overlapping video generation | Code | v2 cell 70 requests 7 min 35 s at 16 fps. Its embedded progress widget shows `0/7280`; the video itself is not embedded or supplied. |
| AE-only sampling makes starry textures without the full image's organization | Author observation | The current v0 snapshot contains later Gencoder code and incompatible legacy sampler cells 55–59; historical sample/version pairing is still needed. |
| Gencoder produces coherent large views, VHS-like texture, and occasional teleportation | Author observation | Existing completed video/stills have not been supplied in this archive. No quantitative teleportation result is preserved. |
| Ambiguous or low-information regions cause teleportation | Hypothesis | Content-based inference provides a possible explanation. The displayed latent mean-square statistic is not an entropy measurement. |
| Unbounded exponential gravity gate, later replaced by bounded gain | Code | v0 cell 30; v2 cell 33. |
| Brightening followed by dark regions, called “neural black holes” | Author observation | The author reports no retained evidence. No explicit overflow-triggered zero reset appears in the notebooks. |
| Numerical instability caused those dark regions | Hypothesis | Plausible from the unbounded gate; the mechanism was not measured. |
| Larger networks/latent hierarchies compress learned structure | Hypothesis / interpretation | Scalar reduction and weight counts can be calculated; end-to-end codec gains require a rate/distortion definition and are not established here. |
| Reused architecture could support recursive self-modeling or general intelligence | Hypothesis / motivation | The implementation demonstrates separate learned operators at two representation levels, not meta-learning or a model of its own parameters. |

## NVD claim ledger

Details and schedule caveats are in [NVD method notes](nvd-notes.md).

| Claim or item | Status | Location / qualification |
| --- | --- | --- |
| Progressive width growth with signed, perturbed corner initialization | Code | Both NVD notebooks, cell 22. Width increments are additive; doubling both dimensions of an internal matrix quadruples its weight count. |
| Temporary thawing of retained weights | Code | Cells 22, 27, 48. Existing gradients ramp from zero toward full scale during the first epoch after growth; Adam is reconstructed at stage transitions. |
| Long loss decline with growth spikes and recovery | Recorded | Growth-only cell 48: displayed minimum 0.288/current 0.381, through approximately epoch 384; the configured 512 epochs are not all preserved. |
| Initial recovery after the first shrink | Recorded | Shrink cell 48: minimum 0.465/current 0.597; the snapshot ends near the start of epoch 97. This does not establish a new post-shrink minimum. |
| Later shrink recovery below a prior minimum | Author observation | Existing later logs/checkpoints have not been supplied. Comparison to an earlier network of the same width needs stage-specific evidence. |
| Negative copying outperformed other initializers; broader freeze/thaw trials did not help | Author observation | The active signed initializer is present, but the supplied outputs do not preserve controlled comparisons of the alternatives. |
| Shrinking proves learning was distributed through the retained network | Hypothesis / interpretation | Recovery after more training alone does not distinguish already-retained information from relearning. |

NVD training curves use the square root of a VGG feature L1 objective, not pixel RMSE. Latent width also grows, and the printed compressed-image count continues to use the original Python `lat_size` value. The shrink training and checkpoint-reconstruction schedules differ by one stage. These issues belong in the historical method account, not in silently revised results.

## Existing assets not yet supplied

The notebooks reference artifacts that are not included in the initial materials. Their existence, version, and recoverability should not be assumed merely because code names them.

- AE and Gencoder `.pth` checkpoints, including optimizer state and any epoch snapshots.
- Serialized training histories, including `v2_log_history.pickle` and `v2_log_history_gen.pickle`.
- Cached latent maps such as `v2_tensor_image_emb.pth` and the exact downloaded source-image file.
- Completed files under `video_out/`, along with selected frames and the generating notebook/checkpoint versions.
- Earlier AE-only sampling versions or retained samples needed to reconstruct that comparison.
- NVD checkpoint/log artifacts and any retained comparisons of initialization, freezing, thawing, or age-dependent learning rates.

For the “neural black holes” observation, the author explicitly reports that no supporting media were saved. It should remain a clearly labeled historical anecdote rather than an item expected to be recovered.

## Publication preparation using existing material

1. Associate each recovered figure or video with its notebook, checkpoint, configuration, and approximate date; preserve uncertain provenance explicitly.
2. Report only quantities already recorded or transparently derived from them. Keep training curves separate from reconstruction, generation, and compression claims.
3. Carry the v0 version mismatch, v2 Gencoder learning-rate issue, metric definitions, and unavailable assets into the manuscript limitations.
4. Keep the ReGAE and NVD empirical accounts separate; use the compression essay to connect their motivations without treating the opinion as experimental proof.

The intended next step is documentation and drafting from this archive, not a requirement for new experiments.

# ReGAE manuscript — revised draft

**Regenerative Autoencoders: Generation and Regeneration through Iterative Denoising**

Version 0.2, October 28, 2026. Prepared for author review and eventual arXiv publication. This is not an arXiv submission or a claim of acceptance at CVPR.

- [Read the revised PDF](regae-draft-v0.2.pdf)
- [Download the revised LaTeX source bundle](regae-draft-v0.2-source.zip)
- [Main LaTeX source](main.tex)
- [References](references.bib)
- Previous version: [v0.1 PDF](regae-draft-v0.1.pdf) and [v0.1 sources](regae-draft-v0.1-source.zip)

## Changes from version 0.1

The paper now introduces ReGAE as a model family whose shape-preserving denoiser can generate from noise and resume reconstruction after perturbation. The conditional form combines an encoder and denoiser; the unconditional form omits the encoder and external code.

The abstract and introduction foreground regeneration and the broader question of what it takes to represent a dataset. The CIFAR-10 example illustrates how another dataset could call for a different hierarchy; it is not a reported experiment. Section 3 describes the general interface, iterative updates, latent tensors, optional losses, configurable U-Net-based architecture, and support for 1D, 2D, or 3D layouts. Figure 1 now depicts conditional and unconditional variants with explicit feedback loops.

Sections 4 and 5 have been rewritten. Pandora's Cluster is presented as a proof of concept using two ReGAE instances, with the Gencoder as the unconditional latent model. VGG-based “perceptive” loss is an application choice rather than a family requirement. Detailed dimensions, noise schedules, optimizer settings, sampling rules, and storage accounting have moved to the appendix. The saved video routine's single-call RGB rendering is distinguished from the family's recurrent capability.

## Build

Use a recent TeX Live or MiKTeX installation with pdfLaTeX, BibTeX, latexmk, TikZ, microtype, and the usual CVPR dependencies. The official CVPR style and bibliography style are included; see [TEMPLATE-NOTICE.md](TEMPLATE-NOTICE.md).

From this directory:

```sh
latexmk -pdf -interaction=nonstopmode -halt-on-error main.tex
```

The output is `main.pdf`. Alternatively, `make` builds and copies it to the versioned PDF filename. A generated `main.bbl` is included for portability. The source ZIP contains `main.tex` at its root, all manuscript sections, bibliography files, style files, and figures. It contains neither training data nor checkpoints.

## Evidence and review

No training or image-generation experiment was run for this revision. The archived setup uses 256-dimensional codes and a 17,644 × 13,422 source image (236.8 MP); those values describe this proof of concept, not the family. Existing video behavior remains author-reported until the saved media are incorporated. The early exponential-gate instability remains an anecdote whose recordings were not retained.

For the next review, the central questions are whether the family definition and regeneration mechanism capture the intended idea, whether the conditional/unconditional distinction is clear, and whether the example supports the broader dataset-modeling argument. Existing stills and video excerpts can then support the qualitative discussion without new experiments. The author list and affiliations still await confirmation; no coauthors or affiliation were inferred.

NVD is reserved for its own manuscript, and the broader intelligence-as-compression argument for the companion essay.

## Figure provenance

`figures/architecture.tex` is an explanatory TikZ diagram of the general family, revised to show recurrence in both variants. It is not experimental evidence. The PNG plots are unchanged bytes extracted from saved outputs of `notebooks/main_v2.ipynb`:

| File | Zero-based notebook cell | SHA-256 |
| --- | --- | --- |
| `figures/ae-training.png` | 39 | `dc1015822103bc67f0d0856acd4637a6e40916b0f04a59d27bc1bff792fd836c` |
| `figures/gencoder-training.png` | 46 | `9d2048b9615b87a9fd8f17d8c5d0f1d8f41920125d1fa8c517d2855c8aaba226` |

The plot captions explain the historical labels: the horizontal count is optimizer updates, and the displayed value is the square root of a training objective. Neither plot reports held-out quality or a controlled regeneration benchmark.

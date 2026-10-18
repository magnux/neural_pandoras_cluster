# ReGAE manuscript — first draft

**Regenerative Autoencoders: Learning Spatial Structure in a Single Astronomical Image**

Version 0.1, October 18, 2026. Prepared for author review and eventual arXiv publication. This is not an arXiv submission or a claim of acceptance at CVPR.

- [Read the PDF](regae-draft-v0.1.pdf)
- [Download the LaTeX source bundle](regae-draft-v0.1-source.zip)
- [Main LaTeX source](main.tex)
- [References](references.bib)

## Build

Use a recent TeX Live or MiKTeX installation with pdfLaTeX, BibTeX, latexmk, TikZ, microtype, and the usual CVPR template dependencies. The official CVPR style and bibliography style are included; see [TEMPLATE-NOTICE.md](TEMPLATE-NOTICE.md).

From this directory:

```sh
latexmk -pdf -interaction=nonstopmode -halt-on-error main.tex
```

The output is `main.pdf`. Alternatively, `make` builds and copies it to the versioned PDF filename. A generated `main.bbl` is included for portability and eventual arXiv source preparation. The ZIP contains source files, bibliography, template files, and figures, including `main.bbl`, with `main.tex` at its root. It contains neither training data nor checkpoints.

## What this draft contains

- The patch autoencoder, multiplicative gate, shifted latent grids, and overlapping Gencoder sampler, described from the archived v2 code.
- A comparison with DiffAE, generative denoising autoencoders, diffusion/SDE/ODE models, and single-image generation.
- A vector architecture schematic and the two original saved training plots.
- Discussion of content-dependent traversal, recursive architecture as a research motivation, and careful model-plus-representation storage accounting.

No training or image-generation experiment was run for this draft. Code dimensions take precedence over recollection: the archived setup has 256-dimensional codes and a 17,644 × 13,422 source image (236.8 MP). Video behavior is explicitly identified as author-reported pending incorporation of existing media. The early exponential-gate instability is an anecdote whose recordings were not retained. No generated visual result has been fabricated or substituted.

## Author review priorities

1. Confirm the author list, preferred name spelling, affiliations, and research chronology. Only the known lead author is listed provisionally; no affiliation or coauthor was inferred.
2. Check whether the method description captures the intended distinction between regeneration, spatial prediction, and architectural recursion.
3. Select existing reconstruction examples and Gencoder video excerpts, with their checkpoint and sampling settings. These will support the qualitative results section.
4. Refine the paper's emphasis and title before tightening it into a submission-ready version.

NVD is reserved for its own manuscript; the broader intelligence-as-compression argument is reserved for the companion essay.

## Figure provenance

`figures/architecture.tex` is a new explanatory TikZ diagram derived from the code. It is not experimental evidence. The PNG plots are unchanged bytes extracted from the saved outputs of `notebooks/main_v2.ipynb`:

| File | Zero-based notebook cell | SHA-256 |
| --- | --- | --- |
| `figures/ae-training.png` | 39 | `dc1015822103bc67f0d0856acd4637a6e40916b0f04a59d27bc1bff792fd836c` |
| `figures/gencoder-training.png` | 46 | `9d2048b9615b87a9fd8f17d8c5d0f1d8f41920125d1fa8c517d2855c8aaba226` |

The captions explain the historical plot labels: the horizontal coordinate is optimizer updates, and the displayed value is the square root of a training objective. Neither plot reports held-out quality.

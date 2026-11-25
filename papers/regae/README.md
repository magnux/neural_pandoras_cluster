# Regenerative Autoencoders: Recursive Modeling through Reusable Denoising

[Read the paper (PDF)](regae.pdf) or start with the [LaTeX source](main.tex). The current draft is v0.8 (November 25, 2026). Git history preserves earlier versions.

## Build the paper

From this directory, run:

```sh
make
```

The build uses pdfLaTeX, BibTeX, and latexmk with TikZ, microtype, and the usual CVPR dependencies. It writes the compiled paper to `regae.pdf` and keeps intermediate files under `build/`. Run `make clean` to remove those intermediates. The CVPR style and bibliography style are included; see the [template notice](TEMPLATE-NOTICE.md).

The manuscript is split into [sections](sections/) and [figure sources and images](figures/); its bibliography is [references.bib](references.bib). The [figure design notes](../../docs/regae-figure-design.md) explain the diagram conventions. The original notebooks are in [notebooks](../../notebooks/).

## Figure provenance

The sliding-tile puzzle uses an original AI-generated illustration of a spiral galaxy (`figures/galaxy-art.jpg`). It is neither a JWST observation nor a ReGAE output. The TikZ source (`figures/galaxy-puzzle.tex`) cuts one image into squares and places them without rotation. Starting from a solved 4 × 4 board with the bottom-right square empty, moving the vacancy left, up, right, up, left, then down gives the arrangement in the paper.

In `figures/architecture.tex`, the same decoder is drawn twice to show two updates to the image while its code stays fixed. `figures/representation-model.tex` separates gencoder training on overlapping observations from subsequent sampling and pixel decoding. The telescope image in this diagram is a display crop, not a neural reconstruction.

`figures/pandora-display-detail.jpg` reproduces the full [1,496 × 1,496 display detail](https://github.com/stephenmwilkins/jwst-first-years-gallery/blob/main/images/gallery/main/weic2305a.jpg) of [Pandora’s Cluster, Abell 2744 (JWST/NIRCam)](https://esawebb.org/images/weic2305a/). No local crop or resampling was applied. The outlines of source and shifted crops are schematic overlays, not a full-resolution training grid. Credit: NASA, ESA, CSA, Ivo Labbe (Swinburne), Rachel Bezanson (University of Pittsburgh); image processing: Alyssa Pagan (STScI).

`figures/notebook-architecture.tex`, `figures/notebook-u.tex`, and `figures/notebook-block.tex` trace the v2 tensors and operations. The tensor drawings label spatial size and channel count; their proportions are schematic. `figures/time-fields.tex` places the same test state on two illustrative fields; its contours and paths are not measured. The PNG plots are unchanged bytes extracted from saved outputs of `notebooks/main_v2.ipynb`:

| File | Zero-based notebook cell | SHA-256 |
| --- | --- | --- |
| `figures/ae-training.png` | 39 | `dc1015822103bc67f0d0856acd4637a6e40916b0f04a59d27bc1bff792fd836c` |
| `figures/gencoder-training.png` | 46 | `9d2048b9615b87a9fd8f17d8c5d0f1d8f41920125d1fa8c517d2855c8aaba226` |

The plot captions explain the historical labels: the horizontal count is optimizer updates, and the displayed value is the square root of a training objective. Neither plot reports held-out quality or a controlled regeneration benchmark.

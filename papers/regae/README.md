# ReGAE manuscript — revised draft

**Regenerative Autoencoders: Recursive Modeling through Reusable Denoising**

Version 0.8, November 25, 2026. Prepared for author review and eventual arXiv publication. This is not an arXiv submission or a claim of acceptance at CVPR.

- [Read the revised PDF](regae-draft-v0.8.pdf)
- [Download the revised LaTeX source bundle](regae-draft-v0.8-source.zip)
- [Sliding-tile galaxy illustration](https://github.com/magnux/neural_pandoras_cluster/blob/main/papers/regae/previews/galaxy-puzzle-v0.5.1.png)
- [Representation-modeling schematic](figures/representation-model.tex)
- [Encoder and full pixel-decoder paths](figures/notebook-architecture.tex)
- [The first U and its projected skip connections](figures/notebook-u.tex)
- [A conditioned convolution block](figures/notebook-block.tex)
- [Main LaTeX source](main.tex)
- [References](references.bib)
- Previous versions: [v0.7 PDF](regae-draft-v0.7.pdf) and [v0.7 sources](regae-draft-v0.7-source.zip); [v0.6 PDF](regae-draft-v0.6.pdf) and [v0.6 sources](regae-draft-v0.6-source.zip); [v0.5.1 PDF](regae-draft-v0.5.1.pdf) and [v0.5.1 sources](regae-draft-v0.5.1-source.zip); [v0.5 PDF](regae-draft-v0.5.pdf) and [v0.5 sources](regae-draft-v0.5-source.zip); [v0.4.1 PDF](regae-draft-v0.4.1.pdf) and [v0.4.1 sources](regae-draft-v0.4.1-source.zip); [v0.4 PDF](regae-draft-v0.4.pdf) and [v0.4 sources](regae-draft-v0.4-source.zip); [v0.3.1 PDF](regae-draft-v0.3.1.pdf) and [v0.3.1 sources](regae-draft-v0.3.1-source.zip); [v0.3 PDF](regae-draft-v0.3.pdf) and [v0.3 sources](regae-draft-v0.3-source.zip); [v0.2 PDF](regae-draft-v0.2.pdf) and [v0.2 sources](regae-draft-v0.2-source.zip); [v0.1 PDF](regae-draft-v0.1.pdf) and [v0.1 sources](regae-draft-v0.1-source.zip)

## Changes in version 0.8

This revision redraws the explanatory figures. Read the family diagram for what changes across denoising steps, the Pandora composition for what moves between the two models, and the implementation figures for the operations and tensor shapes in a single call. The training plots still reproduce the original notebook outputs. The [figure design note](https://github.com/magnux/neural_pandoras_cluster/blob/main/docs/regae-figure-design.md) explains the visual conventions and links to the diagram research that informed them. This revision adds no performance measurements.

## Changes in version 0.7

The discussion treats a patch code as a program for the learned decoder: fixing the code selects an update rule for the reconstruction. It asks what is specified by the encoder, the per-patch code, and the shared decoder, and how changing those choices might change the balance between fidelity to an individual patch and synthesis from shared patterns. These are design choices, not comparisons tested in the archived run.

The paper now separates the reduction from 3,072 input coordinates to 256 latent coordinates from the cost of storing either tensor at a given precision. An alternative code for the latents, or a codec for the complete mosaic, would be a separate result. Revised diagrams trace tensor shapes through the encoder and the denoiser, including the projected additive skips in a U-shaped pass.

## Changes in version 0.6

The abstract introduces patch reconstruction and the mosaic’s large-scale structure. The case study explains how the original sampling observations motivated a second model. The introduction connects reconstruction to a question about intelligence: how could a system extract and organize the information in a dataset? Compression motivates this question; the paper develops ReGAE and its use for meta-modeling.

The gencoder models how the encoder responds across the image. Other layer activations could also serve as observations; the patch code is useful because it is the bottleneck from which the decoder reconstructs each sample. Figure 1 now identifies the denoiser’s role at both levels of the Pandora implementation.

## Changes in version 0.5.1

A small sliding-tile galaxy alongside the abstract illustrates the difference between reconstructing individual patches and learning how they fit together. It is an unnumbered illustration; the experimental figures and their numbering are unchanged.

## Changes in version 0.5

The introduction asks why reconstruction is useful for studying what a model retains. A shuffled photograph separates the information in individual patches from their arrangement. The abstract defines regeneration, identifies the Webb mosaic and reports the qualitative sampling outcome; the cellular lineage, timestep comparison and meta-modeling hypothesis are developed in the main text.

The method and diagrams now distinguish the denoiser architecture from two roles: decoding a patch given its code, and generating a window of codes as the Gencoder. The case study explains why patching the 236.8-megapixel mosaic leaves a spatial prediction problem and how a fractional shift makes the Gencoder predict new codes throughout a window. The discussion, conclusion and appendices use the same terminology. No experiments or evidence were added.

## Changes in version 0.4.1

Corrected the author's intended concept to **meta-modeling** throughout the abstract, introduction, method, discussion, conclusion, and current planning notes. Section 5.1 now develops recursive models of learned representations and the broader AGI hypothesis. The learning-to-learn digression, MAML comparison, and two meta-learning references have been removed. The construction, experiments, and figures are unchanged.

## Changes in version 0.4

The main text has been rewritten around the [accepted narrative direction](https://github.com/magnux/neural_pandoras_cluster/blob/main/docs/regae-narrative-plan.md). It opens with what learning a dataset requires, explains the ReGAE family, develops composition over learned representations, and uses Pandora as a worked case. Detailed comparisons now follow the method and application; implementation details and the historical gravity anecdote remain in the appendices.

The research aim is recursive modeling rather than task-specific benchmark performance. The abstract, introduction, method, discussion, and conclusion now connect modeling data to modeling the composite system's learned representations. That revision included a meta-learning interpretation that the author subsequently corrected to meta-modeling in v0.4.1; its associated discussion and references have been superseded.

A new schematic shows how encoder-generated code fields become data for the Gencoder and how their updates condition image reconstruction. It distinguishes architectural reuse across levels from recurrent state reuse within a level. The existing contour-band comparison is retained and moves to the generative-dynamics section.

## Changes in version 0.3.1

Figure 2 now uses matching contour geometry and a matching noise-to-target path in both panels. The time-conditioned panel colors outer, middle, and inner bands for successive sampling stages; the ReGAE panel is preserved. The caption and adjacent sentence explain the schematic comparison and distinguish these illustrative bands from disjoint domains of time-conditioned fields. All other scientific content is unchanged.

## Changes in version 0.3

The abstract and introduction now introduce regeneration through neural cellular automata: growth produces a pattern, persistence maintains it, and regeneration repairs an already developed pattern after damage. The text separates this biological analogy from NCAM's encoder-conditioned growth-program lineage.

The method explicitly defines direct state prediction and output feedback without a timestep or corruption-level input. Conditioning on an encoded sample is distinguished from conditioning on time. The discussion compares clean-state, noise, and velocity prediction, including diffusion's alternative parameterizations and the role of a sampling or integration rule.

Figure 2 compares time-indexed fields over the same state space with a single time-unconditioned ReGAE map. It is a conceptual diagram, not a measured landscape. A new mathematical appendix gives the squared-error conditional-mean interpretation with hidden corruption strength, a sufficient local repair condition, and an algebraic Euler connection. The text distinguishes these interpretations from demonstrated properties of the archived models. Stanley H. Chan's diffusion tutorial and Flow Matching have been added to the references.

## Changes in version 0.2

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

For review: is it clear how a fixed code changes the decoder’s update rule, what the gencoder can learn by observing those codes, and what the 12:1 coordinate count does and does not establish? Existing stills and video excerpts can then support the qualitative discussion without new experiments. The author list and affiliations still await confirmation; no coauthors or affiliation were inferred.

NVD is reserved for its own manuscript, and the broader intelligence-as-compression argument for the companion essay.

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

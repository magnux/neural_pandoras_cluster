# Neural Pandora's Cluster

Exploratory research on **Regenerative Autoencoders (ReGAE)** and **Neural Vapor Deposition (NVD)**, using a large JWST image of Pandora's Cluster as a shared learning problem.

ReGAE is a family of models for generation and regeneration through iterative denoising. A conditional ReGAE combines an encoder with a shape-preserving denoiser; an unconditional variant omits the encoder and external code. The Pandora's Cluster experiments compose two instances to learn image samples and spatial relationships among their representations. NVD investigates progressive parameter-block growth and later structured removal. A companion essay will examine learning through the lens of compression.

This repository is a research archive and publication workspace. It preserves four original notebook snapshots and organizes the existing evidence for two research papers and one opinion essay. The [revised ReGAE paper draft (v0.3)](papers/regae/regae-draft-v0.3.pdf) is available in PDF, with [LaTeX sources and build instructions](papers/regae/). Selected generated images, videos, and checkpoints will be added separately. No new training experiments were run to prepare the archive or manuscript drafts.

## Research directions

### Regenerative Autoencoders

A conditional ReGAE encodes a sample into a latent tensor and repeatedly applies a denoiser to a data state. The denoiser has matching input and output shapes, so it can generate from noise and resume reconstruction after perturbation. Its architecture, loss, and data dimensionality are configurable. An unconditional ReGAE retains the recurrent denoiser without an encoder-derived conditioning code.

In the Pandora proof of concept, the first instance learns image-patch reconstruction. A second, unconditional instance—the **Gencoder**—models grids of latent patches and predicts a small spatial displacement. Recurrent, overlapping sampling generates larger image regions without global source-coordinate conditioning. This two-level spatial setup is one application of the family.

The attached v0 and v2 configurations encode a 32 × 32 RGB patch into 256 values. The v2 Gencoder processes 32 × 32 latent patches, corresponding to 1024 × 1024 decoded pixels. Its saved source-image dimensions are 17,644 × 13,422, approximately 237 megapixels.

Architectural reuse across representation levels motivates a recursively extensible hierarchy. The current implementation has two separately trained levels; stronger self-modeling and meta-learning interpretations remain research hypotheses. The notebooks also contain a learned multiplicative feature gate, called **gravity**, whose bounded formulation replaced an earlier exponential gate.

### Neural Vapor Deposition

NVD expands a trained network by adding parameter blocks initialized from negatively correlated copies of learned blocks, together with small random cross-connections. Fixed input and output dimensions receive special treatment. The shrinking variant removes the most recently deposited channel groups and continues training.

These snapshots implement additive increments in width. The research notes distinguish this implementation from the broader proposal of arbitrary growth factors, and distinguish stored training evidence from the author's recollection of additional experiments.

### Learning as compression

The connecting essay will distinguish compact representations, total model-and-data description length, and practical storage savings. Smaller latent vectors or fewer network parameters do not alone establish a competitive image codec. The comparison must account for precision, required codes and model weights, and reconstruction fidelity.

## Repository guide

| Path | Contents |
| --- | --- |
| [papers/regae/](papers/regae/) | ReGAE revised draft (v0.3), LaTeX sources, references, figures, and prior versions |
| [notebooks/](notebooks/) | Four original notebook snapshots, provenance notes, and SHA-256 checksums |
| [docs/publication-roadmap.md](docs/publication-roadmap.md) | Publication scope, outlines, proposed figures, and related work |
| [docs/regae-notes.md](docs/regae-notes.md) | ReGAE architecture, DiffAE comparison, gravity, and code observations |
| [docs/nvd-notes.md](docs/nvd-notes.md) | NVD operators, schedules, evidence, and implementation caveats |
| [docs/evidence-inventory.md](docs/evidence-inventory.md) | Claim status and the existing material needed to substantiate it |
| [assets/README.md](assets/README.md) | How figures, videos, checkpoints, and their provenance will be recorded |

## Evidence and status

The archive currently includes notebook code, saved text outputs, and several training-loss plots. It does not yet include the author's separate checkpoints, generated images, or videos. Coherent generation, apparent navigation between image regions, and early instability described as "neural black holes" are author-reported observations pending the availability of their supporting material; recordings of the latter were not retained.

The notebooks are historical working snapshots, not a verified end-to-end reproduction package. Known inconsistencies are documented rather than silently corrected. Future code cleanup should preserve a clear distinction between historical runs and changed implementations.

The first publication will describe ReGAE, followed by NVD and the connecting compression essay. See the [publication roadmap](docs/publication-roadmap.md) for the bounded plan using existing experiments.

## Background and image source

- [Neural Cellular Automata Manifold](https://arxiv.org/abs/2006.12155), Alejandro Hernandez Ruiz, Armand Vilalta, and Francesc Moreno-Noguer: the preceding NCAM work.
- [Diffusion Autoencoders](https://diff-ae.github.io/): a central comparison for a learned encoder, conditional denoising decoder, and latent generative model.
- [Original STScI image URL used by the notebooks](https://stsci-opo.org/STScI-01GQQF9WVPFVMCVHRZY54N2TAR.png). The source image is downloaded by the notebooks and is not bundled here. Full image provenance and credit will accompany publication figures.

The reference map in the roadmap also covers generative denoising autoencoders, single-image generation, diffusion and ODE connections, network expansion, pruning, and compression.

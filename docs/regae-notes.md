# ReGAE: method and publication notes

These are retrospective working notes, not a paper or a new experimental report. They distinguish the archived implementation from the author's recollections and proposed interpretations. Notebook cell numbers below are **zero-based indices**, including Markdown cells. No training or sampling was run for this review.

## Research question and scope

Regenerative AutoEncoders (ReGAE) investigate how a model can retain local appearance and larger spatial organization from one large astronomical image. The motivating question is constructive: even memorizing a rich dataset requires a mechanism that can represent its information and reconstruct or generate its structure.

The proposed narrative is a hierarchy: a patch autoencoder learns local appearance; a second denoising network, the **Gencoder**, learns relationships among patch encodings while predicting a small displacement. Recurrent, overlapping updates provide a content-guided generation process. Whether this establishes extrapolation depends on the definition and supporting evaluation; the archived results do not settle the general memorization-versus-generalization debate.

The author developed ReGAE as an extension of [Neural Cellular Automata Manifold (NCAM)](https://arxiv.org/pdf/2006.12155), with revised conditioning and regeneration objectives. The current U-Net/attention implementation should be described directly rather than assumed to retain every property of a strictly local cellular automaton.

## What the archived implementation does

The references in this section are to [main_v2.ipynb](../notebooks/main_v2.ipynb), unless specified otherwise.

| Part | Implementation | Cells |
| --- | --- | --- |
| Image | Saved shape `3 × 13422 × 17644`, or **236,817,768 pixels (about 237 MP)**; source URL points to a PNG | 9–11 |
| Patch representation | `32 × 32 × 3` input; one **256-dimensional** latent vector; `n_filter=64` | 11, 32, 39 |
| Encoder | Convolutional downsampling and linear reduction; no VAE KL term, vector quantizer, or fitted latent prior in this stage | 32, 39, 41 |
| Decoder | Convolutional inlet, normalized U-Net, multiplicative gate, unnormalized U-Net, RGB projection; latent-conditioned blocks | 26, 29, 33 |
| AE objective | Corrupt a patch by mixing it with Gaussian noise; predict the clean patch, conditioned on its encoding, with VGG16 feature L1 loss | 35, 37, 41 |
| Gencoder | Another `Decoder` instance with 256 input/output channels and a `32 × 32` latent window; separately learned weights | 46 |
| Latent data | Four interleaved encodings of the image, saved shape `4 × 256 × 419 × 551` | 47 |
| Gencoder objective | Corrupt an origin latent window; predict its clean counterpart at the next offset, using L1 loss | 49 |
| Video sampler | Overlap and average latent-window predictions, decode patches, crop the assembled image to `1920 × 1080` | 54, 70 |

For a clean patch `x`, the first-stage objective has the form

```text
z = E(x)
u = alpha * epsilon + (1 - alpha) * x
loss_AE = perceptual_L1(D(u, z), x)
```

The noise mixture uses a geometric schedule with seven nominal calls and randomly sampled intermediate fractions. Training predicts the clean target in one call; the displayed reconstruction code also uses one decoder call. There is no explicit noise-time input. The paired positive/negative noise samples in v2 balance sampled noise, but should not be described as establishing an unbiased estimator for every nonlinear quantity.

For the second stage, `G(alpha * epsilon + (1-alpha) * z_origin)` predicts `z_target`. The target uses the next of four fractional-patch offsets, wrapping into the next latent-grid position. This corresponds to approximately **eight image pixels in both axes** per update; integer crop rounding prevents an everywhere-exact eight-pixel statement. All target encodings use shifted image patches, rather than simply reusing most tokens and appending a border. A `32 × 32` latent window decodes to `1024 × 1024` pixels.

The Gencoder has learned **patch-relative positional tensors** (`wave_pos`, cell 26), but no input specifying global coordinates in the original image. Thus “without global coordinate conditioning” is more accurate than “without positional encoding.”

## Gravity gate and historical observations

In [main_v0.ipynb](../notebooks/main_v0.ipynb), cell 30, the first U-Net outputs feature tensors `h` and `g`; the gate is `h * exp(g)`. This multiplier is unbounded. In v2, cell 33, it becomes

```text
h * (clip(1 + g / 3, 0, 2) + 0.0001)
```

The new multiplier lies between `0.0001` and `2.0001`. It bounds this gate's gain, not the entire network or its recurrent trajectory.

The author recalls early recurrent samples developing bright regions followed by large dark regions, informally called **“neural black holes.”** No recordings or measurements of that behavior were retained. Numerical instability is a possible explanation, not an established mechanism. Neither notebook implements an overflow-triggered zero reset. Image quantization/clipping is present; the `zero_()` call in the final video loop clears an overlap accumulator each frame. The gravity name is an analogy, not a claim to simulate physical gravitation.

## Relation to diffusion, ODEs, and DiffAE

The first stage is a conditional denoising autoencoder. Repeated application of the Gencoder with injected noise defines a discrete stochastic dynamical system. The code does not specify a score-matching objective, reverse SDE, continuous-time velocity field, or ODE solver. These are useful connections to investigate in the writing, rather than identities already established by the implementation.

[Diffusion Autoencoders (DiffAE)](https://openaccess.thecvf.com/content/CVPR2022/html/Preechakul_Diffusion_Autoencoders_Toward_a_Meaningful_and_Decodable_Representation_CVPR_2022_paper.html) are an important comparison. Their representation pairs a **512-dimensional nonspatial semantic vector** with a **spatial stochastic code `x_T`**, decoded by a time-conditioned DDIM. Their unconditional generation stage learns a latent diffusion prior over semantic vectors, using an MLP architecture ([official project](https://diff-ae.github.io/); [latent-model configurations](https://github.com/konpatp/diffae/blob/master/templates_latent.py)). ReGAE's Gencoder instead operates over a spatial grid of patch vectors and learns a displaced target. DiffAE must not be reduced to a single 512-number code, and the presence of a second learned latent model is not by itself a novelty claim.

The author also sees the shared decoder architecture as a route toward recursive modeling, self-modeling, and eventually more general intelligence. What is implemented here is a **two-level representation hierarchy** using separate parameters. The code does not model its own weights or learning process, learn an optimizer, or perform meta-training across tasks. Recursive self-modeling and AGI remain research motivations.

## Recorded quantities and reporting cautions

| Quantity | Saved output | Reference |
| --- | ---: | --- |
| Encoder parameters | 4,673,752 | v2 cell 39 |
| Decoder parameters | 50,986,296 | v2 cell 39 |
| Gencoder parameters | 85,479,680 | v2 cell 46 |
| AE plotted loss, minimum / current | 0.389 / 0.450 | v2 cell 39 |
| Gencoder plotted loss, minimum / current | 0.235 / 0.252 | v2 cell 46 |

The plotted values are **square roots of per-batch training losses**, not RMSE or held-out scores. Logging happens per optimizer step, although the plot labels say “epoch.” They should not be promoted to evaluation metrics. The video overlay `z.pow(2).mean()` measures mean squared latent activation, **not entropy**.

The 256-dimensional code gives a 12-fold scalar-count reduction from 3,072 RGB values. With uint8 RGB and FP32 latents, the patch payload reduction is only threefold before accounting for model weights. The recorded parameter counts imply about **222.64 MB of AE FP32 weights** and **341.92 MB of Gencoder FP32 weights**. The author's recalled 890 MB and 1.4 GB sizes are consistent with full AMSGrad checkpoints containing weights plus three optimizer-state tensors; that explanation is inferred from code and arithmetic, not inspection of the missing checkpoints. These facts support a representation/compression discussion, not a demonstrated improvement over a JPEG codec.

## Archive issues to preserve in the account

- **v0 provenance:** its current cells 41–44 already contain a latent Gencoder, despite the author's description of an earlier AE-only phase. Legacy sampling cells 55–59 pass RGB tensors to that 256-channel network and use conditioning inconsistent with the current decoder. Identify the historical version behind each visual before presenting a baseline comparison.
- **Gencoder learning rate:** v2 cell 49 changes `optimizer.param_groups`, rather than `gen_optimizer.param_groups`. The written decay schedule therefore does not update the Gencoder optimizer.
- **Claim boundaries:** use “local denoising/reconstruction regularities” rather than calibrated “patch probabilities.” Large coherent samples, VHS-like texture, and content-dependent teleportation are author observations until existing media are associated with a precise run. Low information content causing teleportation is a hypothesis.
- **Publication scope:** reuse existing material, document these limitations, and preserve the historical notebooks. This initial release does not add experiments or a full manuscript.

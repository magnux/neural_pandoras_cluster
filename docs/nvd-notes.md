# Neural Vapor Deposition: method and evidence notes

These are preparation notes for a future paper, not a manuscript or a new experimental report. They describe the archived [main_nvd.ipynb](../notebooks/main_nvd.ipynb) and [main_nvd_shrink.ipynb](../notebooks/main_nvd_shrink.ipynb) sources and their saved outputs. No training or new experiments were run for this audit. Cell references below are zero-based and include markdown cells.

## Candidate contribution

Neural Vapor Deposition (NVD) progressively widens a trained network by adding channel groups and their connections. Newly added blocks inherit a sign-reversed, perturbed version of an existing learned block. A complementary shrink operation removes the most recently deposited channel groups. The supplied case study applies these operations to a ReGAE encoder and conditional decoder trained on patches from the Pandora's Cluster image.

The crystal-deposition analogy motivates the method. It should be presented as an analogy, not as a physical model or as evidence of improved optimization. Ordinary neural-network training is not generally called "neural annealing"; the notebooks use Adam.

## Growth and initialization

`NVD_Module` stores each layer's original input/output growth increments. Each call to `grow()` adds those same increments to unfixed dimensions. For an internal matrix with base dimensions `N × M`, the successive shapes are `N × M`, `2N × 2M`, `3N × 3M`, and so on. Doubling both dimensions quadruples the matrix's weight count. Subsequent deposits in these notebooks are additive width increases, not repeated doubling. Fixed-interface matrices and biases have different parameter scaling. Arbitrary growth factors are an extension of the idea, not a configurable feature demonstrated here. [Both notebooks, cells 22–26.]

For each parameter tensor, let `C` be its trailing corner with the original base parameter shape. With independent standard Gaussian entries `ε`, the deposited corner is initialized exactly as:

```text
C_new = -(0.9 * C + 0.1 * std(C) * ε)
```

Thus the inherited block is generally the most recently deposited corner, rather than always the initial seed block. The rest of the padded tensor's zero-valued entries receive Gaussian noise scaled by `0.01 * std(C_new)`. This includes the newly exposed cross-connections; the implementation's zero-value mask can also affect exact zeros in retained weights. The resulting expansion is not function preserving. [Cell 22.]

The author recalls trying random initialization, matched mean/standard deviation, Fourier-based initialization, and negative copying with noise. Only the last is active in the supplied sources; their retained outputs do not contain an initializer comparison.

RGB interfaces remain fixed through `fix_in=True` on input adapters and `fix_out=True` on the decoder's output convolution. Padding determines which dimensions grow. Gradient masks have a separate role: controlling updates to existing weights. One implementation detail to retain in a reproducibility audit is that tensors with no growing dimension, such as the fixed output bias, still pass through the signed corner initializer. [Cells 22–24, 34, 38–39.]

## Optimization and temporary freezing

The configuration uses seed 4202, batch size 64, Adam at `1e-4`, betas `(0.5, 0.9)`, and `amsgrad=True`. A fresh Adam optimizer is constructed after every growth or shrink, so moment estimates are reset. [Cells 46 and 48.]

After growth, a binary mask marks all preexisting entries. During the first epoch of the new stage, their gradients are multiplied by a factor increasing from zero toward one. New entries receive unscaled gradients. After shrink, masking is skipped. The active code therefore implements one-epoch thawing, not separate learning rates for every deposition age. Gradient scaling under Adam is not generally equivalent to scaling its learning rate. The author's recalled broader freeze/thaw and age-dependent learning-rate trials remain historical observations without retained comparisons in these files. [Cells 22, 27, 48.]

## Shrink operation and schedule

`shrink()` crops the leading portion of each parameter tensor, removing the latest appended rows/channels, columns/channels, and their connections. Normalization group counts decrease accordingly. Removal follows reverse deposition order rather than weight importance. There is no active distillation or explicit transfer from the removed blocks; proposed mixing code is commented out. [Shrink notebook, cells 22, 25, 27.]

Both notebooks configure 512 epochs, with a stage lasting 32 epochs. Initial filter width is 8 and initial latent width is 32. The active training schedule is:

| Training epochs | Growth-only width in base units | Grow/shrink width in base units |
| --- | ---: | ---: |
| 0–31 | 1 | 1 |
| 32–63 | 2 | 2 |
| 64–95 | 3 | 3 |
| 96–127 | 4 | 2 |
| 128–159 | 5 | 3 |
| 160–191 | 6 | 4 |
| 192–223 | 7 | 5 |
| 224–255 | 8 | 4 |

The first shrink enters the fourth training stage, after two growth transitions from the seed. Later shrink events have three growth events between them. Counting the seed as a deposition resolves some ambiguity in the informal "three deposits, then shrink" description. [Cells 46 and 48.]

There is an off-by-one discrepancy between the shrink notebook's training and checkpoint-reconstruction schedules:

```python
# Training transition, cell 48
(((training_step // len_epoch) // dep_epochs) + 1) % 4 == 0

# Checkpoint architecture reconstruction, cell 46
(n_epochs // dep_epochs) % 4 == 0
```

The archive should preserve this historical source. Any corrected runnable version should document the correction separately; the two branches cannot be assumed to reconstruct identical stage histories after shrinking.

## What the retained evidence shows

The loss sums L1 differences between features from four frozen pretrained VGG16 blocks. Logged batch values are the square root of this perceptual objective. They are **not pixel RMSE**. The progress display instead uses the square root of an exponentially smoothed objective. Although the plot axis says "epoch," the logger receives a value every optimizer update: 3,607 updates constitute one configured epoch. Reloaded historical values are clipped to `[0, 1]` before plotting. [Cells 41, 46, 48.]

| Saved output | Minimum displayed loss | Current displayed loss | Interpretation |
| --- | ---: | ---: | --- |
| Growth-only, final cell 48 | 0.288 | 0.381 | Long downward trajectory with growth spikes and recovery, through approximately 384 epochs |
| Grow/shrink, cell 46 | 0.465 | 0.571 | Snapshot before the first recorded shrink |
| Grow/shrink, final cell 48 | 0.465 | 0.597 | Initial recovery after the shrink around epoch 96; snapshot stops near the start of epoch 97 |

The growth-only output records a subsequent expansion from 10,877,808 to 12,722,346 encoder parameters and from 111,115,731 to 130,209,849 decoder parameters, then an interruption at the start of epoch 384. The configured 512-epoch run is not fully represented. Neither NVD notebook retains outputs from its later Gencoder or image-testing sections.

The attached shrink snapshot does not establish a post-shrink minimum below the preceding all-time minimum. The author reports later rapid recovery and improvement; those observations can be described as author-reported, or substantiated with existing later logs and checkpoints if available. A comparison with the earlier model of the same width also requires appropriate stage-specific evidence. This limitation does not contradict results beyond the saved snapshot.

Even when recovery is established, recovery after further training does not by itself identify how much information was already retained versus relearned. It supports the practical usefulness of the retained smaller network, not a proof that removed blocks transferred their knowledge into it.

## Compression accounting

Growth expands the latent bottleneck as well as hidden widths: the encoder's output dimension is not fixed, so stage width `k` has latent width `32k`. The printed "Compressed Image params" calculation continues to use the unchanged Python variable `lat_size=32`; after growth, it is not the actual latent-map size. Loss improvements therefore accompany both increased model capacity and increased representation capacity. [Cells 23, 38, 46.]

Shrinking reduces parameter count relative to retaining or further growing the expanded architecture. The supplied record does not establish better rate–distortion performance, a smaller total image codec, faster training than a matched baseline, or superiority over training the final small architecture directly. Those should remain open questions rather than promised new experiments.

## Related work and paper positioning

- [Net2Net: Accelerating Learning via Knowledge Transfer](https://arxiv.org/abs/1511.05641), Chen, Goodfellow, and Shlens: function-preserving widening/deepening transfers a trained network into a larger architecture. NVD should explain its signed corner initialization and resulting function change in relation to this established line of work.
- [NeST: A Neural Network Synthesis Tool Based on a Grow-and-Prune Paradigm](https://arxiv.org/abs/1711.02017), Dai, Yin, and Jha: grows connections using gradients and prunes using magnitude, starting from a sparse seed. NVD's dense block deposition and chronological removal offer a different mechanism within the broader grow-and-prune literature.

These references establish relevant precedents, not an exhaustive novelty review. A defensible paper would present the specific operator, implementation, and historical trajectories as an exploratory case study. Claims that growth itself, growth followed by shrinkage, or crystal-inspired terminology establish novelty would be too broad.

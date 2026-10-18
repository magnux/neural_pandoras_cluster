# Publication roadmap

Publish two self-contained research preprints, followed by an opinion essay that connects them through compression. The aim is to preserve the methods and the evidence already produced. This project does not require new training runs, ablations, benchmarks, or generated samples.

## Writing order and deliverables

1. **ReGAE paper:** establish the architecture, the shift from local reconstruction to spatial latent prediction, and the observed sampling behavior.
2. **NVD paper:** describe growth and shrinkage precisely, then present the retained training evidence and its limits.
3. **Compression essay:** build the broader argument around the two completed technical accounts; adapt it into a short LinkedIn post linking to the eventual public papers and code.

For each paper, first reconcile prose with the notebooks, then assemble figures from retained outputs, write the first draft, and review claims and attribution. Prepare arXiv source and supplementary material only after author review. Missing historical assets remain declared gaps rather than reasons to launch new experiments. Preparing these materials does not itself publish them.

## 1. ReGAE research paper

**Working title:** *Regenerative Autoencoders: Learning and Traversing Spatial Structure in a Single Astronomical Image*

**Central account:** an encoder and conditional denoising decoder learn local image patches; a second model, the Gencoder, learns spatial relationships among their latent representations. Predicting a slightly shifted patch makes the second stage a recurrent spatial predictor. Overlapping decoded outputs support a continuous visual traversal of the learned image distribution.

**Proposed outline:**

1. Motivation and relationship to Neural Cellular Automata Manifold (NCAM).
2. Patch encoder, regenerative decoder, corruption process, loss, and sampling updates.
3. The v0 sampling strategies and the distinction between local appearance and larger spatial organization.
4. The v2 latent hierarchy: patch geometry, offset target, overlapping synthesis, and content-based traversal.
5. Retained results and author observations, including visual coherence, persistent noise, and apparent relocation between familiar regions.
6. Connections to denoising autoencoders, DiffAE, latent diffusion, and discrete/continuous dynamical systems; limitations and future directions.

**Claims to develop:** document the implemented mechanism and the qualitative behavior supported by retained material. Present exact image reconstruction, memorization, distribution learning, and new-looking synthesis as different objectives. An apparently unfamiliar output is not evidence of extrapolation beyond the training distribution. The proposed explanation of relocation through ambiguous content is a hypothesis; do not call it a measured entropy effect without a defined statistic and retained measurements.

**DiffAE is a central comparison.** It already combines an image encoder with a conditional diffusion decoder and fits another diffusion model to the semantic latent distribution. ReGAE's comparison should therefore focus on its regenerative updates, spatial patch hierarchy, shifted latent targets, and traversal behavior. Neither an encoder-conditioned denoiser nor a second latent generative model is, by itself, the contribution. Specify the actual noise process and update equations before drawing an equivalence to diffusion or a probability-flow ODE.

Reusing the same architectural family recursively is an important design motivation. It suggests a route toward systems that model increasingly abstract representations; these experiments do not demonstrate self-modeling or meta-learning. Describe the change from an exponential to a bounded gravity gate using the archived code. Present the reported “neural black holes” as author-recalled, unretained observations, clearly separated from documented results. Do not invent plots, frequency estimates, or a causal mechanism for them.

## 2. NVD research paper

**Working title:** *Neural Vapor Deposition: Progressive Block Growth and Shrinkage of Neural Networks*

**Central account:** enlarge a trained network by adding parameter blocks and their connections, initialize the additions using information from the existing network, and continue optimization. Later remove deposited blocks and retrain the surviving network. Crystal growth supplies the motivating analogy; the contribution must be the specified algorithm and recorded behavior.

**Proposed outline:**

1. Motivation: learning a useful small network before allocating further capacity.
2. Block construction, signed-copy initialization, perturbations, and new connections.
3. Fixed input/output dimensions, padding and masks, and actual growth schedule.
4. Optional freezing, thawing, and age-dependent learning rates; distinguish supported code paths from retained comparisons.
5. Shrinkage by deposition order, continued training, and the recorded loss curves.
6. Relationship to Net2Net, network morphism, and grow–prune methods; limitations.

**Claims to develop:** describe signed-copy initialization as the author's preferred result from exploratory trials unless retained comparisons support a stronger ranking. Growth and shrinking are demonstrated procedures, not evidence of general superiority over training from scratch or established pruning methods. Recovery after shrinking and further optimization supports the usefulness of the training trajectory; it does not alone establish where information resided before removal or isolate the benefit from additional training.

Report width, number of deposits, and total parameter count separately. Expanding an internal dense matrix from $N\times M$ to $2N\times2M$ produces four times as many entries. Do not describe this as merely doubling parameters, or assume that every code path uses this growth factor. Check signed-copy behavior and symmetry arguments against the actual nonlinearities. Ordinary gradient-based training is not generally called “neural annealing”; retain annealing and vapor deposition as explanatory metaphors.

## 3. Compression opinion essay and LinkedIn adaptation

**Working title:** *What Does It Mean for a Neural Network to Compress What It Learns?*

**Thesis:** learning can be examined as the construction of reusable descriptions. ReGAE separates local appearance from larger spatial dependencies; NVD asks how the capacity carrying such descriptions can grow and contract. This is a useful lens on intelligence, not a demonstrated equivalence between compression and intelligence.

| Sense of compression | Application here | What must be counted or qualified |
|---|---|---|
| Representation compression | Map a pixel patch to a smaller latent vector. | Scalar dimension, numeric precision, reconstruction error, and decoder cost. A bottleneck is not automatically a smaller file. |
| Statistical description of data | Learn regularities that can help describe patches and their spatial arrangement. | Model description plus any latents, residual information, ordering, and decoding requirements. Sampling a similar image is different from encoding the original. |
| Model compression | Retain useful behavior with fewer network parameters through shrinking. | Remaining parameter count and storage at an identified quality level; compute and storage are separate costs. |

**Proposed outline:** begin with the challenge hidden inside “just memorization”; explain the three meanings above; use ReGAE to discuss hierarchical descriptions; use NVD to discuss temporary capacity and consolidation; end with open questions about prediction, abstraction, and the limits of the compression lens. Introduce minimum description length as context, not a theorem proved by these experiments.

Use explicit storage arithmetic. A $32\times32\times3$ uint8 patch occupies 3,072 bytes; a $d$-component FP32 latent occupies $4d$ bytes before overhead. The scalar reduction $3072/d$ therefore differs from the byte reduction $3072/(4d)$. Quote the latent width for each actual run rather than treating recalled widths as universal settings.

The reported 890 MB and 1.4 GB files are historical size estimates, not verified inference payloads. An FP32 weight payload is approximately $4P$ bytes for $P$ parameters, whereas a training checkpoint may also contain optimizer moments and other state. Inspect what was saved before attributing the whole file size to weights. Even verified model sizes cannot be compared to a roughly 200 MB JPEG as a successful codec without accounting for decoder information and reconstruction quality. No quantized size or quality result is claimed without an existing measurement.

The LinkedIn version should contain one motivating question, one concrete example from each project, one limitation, and links to the public artifacts when available. It should invite discussion without claiming a proof that intelligence is compression or that neural networks cannot extrapolate.

## Figures from existing evidence

| Figure | Source and treatment |
|---|---|
| ReGAE hierarchy and shifted target | Draw a schematic directly from `main_v0.ipynb` and `main_v2.ipynb`; label spatial sizes, latent width, and offsets for the relevant configuration. This is an explanatory diagram, not a new result. |
| ReGAE training behavior | Extract the retained v2 loss images. Preserve axes and identify what each recorded run measures; do not reconstruct missing numerical histories from the image as exact data. |
| Reconstruction and sampling examples | Use retained images or frames only when recovered and attributable to a run. v0 currently contains no embedded image outputs. Do not substitute newly sampled examples. |
| Coherent traversal and apparent relocation | Select frames from an existing video if supplied. Caption the visible behavior and keep the proposed explanation separate. Omit this results figure if no recording is retained. |
| NVD block growth and fixed I/O | Draw a code-grounded schematic showing old blocks, new blocks, coupling, and masks, followed by the corresponding shrink operation. |
| NVD growth/shrink loss traces | Extract the saved images in the two NVD notebooks. Annotate deposition/removal events only where their mapping to the plotted run is established. |
| Compression accounting | Use a small table or schematic of representation, data-description, and model costs. Label arithmetic examples and historical estimates explicitly. |

## Selected primary references

This is a focused starting bibliography, not an exhaustive novelty assessment. Dates distinguish preprints from venue publication where useful.

| Reference | Relation to these publications |
|---|---|
| [Hernandez Ruiz et al., NCAM, 2020 preprint / CVPR 2021](https://arxiv.org/abs/2006.12155) | Direct architectural and conceptual starting point; explain which conditioning and regenerative mechanisms changed. |
| [Bengio et al., Generalized Denoising Auto-Encoders as Generative Models, 2013](https://papers.neurips.cc/paper/5023-generalized-denoising-auto-encoders-as-generative-models.pdf) | Alternating corruption/denoising and walkback sampling; its convergence assumptions are not automatically inherited. |
| [Alain & Bengio, What Regularized Auto-Encoders Learn, JMLR 2014](https://www.jmlr.org/papers/v15/alain14a.html) | Connects regularized reconstruction to density scores under specified assumptions. |
| [Ho et al., Denoising Diffusion Probabilistic Models, 2020](https://arxiv.org/abs/2006.11239) | Noise-conditioned denoising and progressive lossy decompression; compare objectives and sampling equations. |
| [Song et al., Score-Based Generative Modeling through SDEs, 2020 / ICLR 2021](https://arxiv.org/abs/2011.13456) | Reverse-time SDE and probability-flow ODE; informs a connection, not an equivalence established here. |
| [Preechakul et al., Diffusion Autoencoders, 2021 / CVPR 2022](https://arxiv.org/abs/2111.15640) ([official project](https://diff-ae.github.io/)) | Central comparison: semantic encoder, conditional diffusion decoder, and a learned latent sampler; contrast spatial shifted prediction. |
| [Rombach et al., Latent Diffusion Models, 2021 / CVPR 2022](https://arxiv.org/abs/2112.10752) | Generative modeling in an autoencoder's latent space; compare hierarchy and computational purpose. |
| [Shaham et al., SinGAN, ICCV 2019](https://arxiv.org/abs/1905.01164) | Single-image multiscale patch distributions and synthesis; prevents overclaiming the single-image setting. |
| [Chen et al., Net2Net, 2015 / ICLR 2016](https://arxiv.org/abs/1511.05641) | Function-preserving expansion, replication, and symmetry-breaking noise; distinguish NVD's signed block construction. |
| [Wei et al., Network Morphism, ICML 2016](https://proceedings.mlr.press/v48/wei16.html) | Changes architecture while preserving the original function; NVD need not satisfy that guarantee. |
| [Dai et al., NeST, 2017 / IEEE Transactions on Computers 2019](https://arxiv.org/abs/1711.02017) | Seed networks with gradient-guided growth and magnitude pruning; contrast deposition and chronological removal. |
| [Han et al., Learning both Weights and Connections, NeurIPS 2015](https://arxiv.org/abs/1506.02626) | Established train–prune–retrain compression; recovery after removal is not itself a new principle. |
| [Grünwald, A Tutorial Introduction to the Minimum Description Length Principle, 2004](https://arxiv.org/abs/math/0406077) | Formal context for counting both model description and unexplained data, rather than latent dimension alone. |
| [Rissanen, Modeling by Shortest Data Description, 1978](https://research.ibm.com/publications/modeling-by-shortest-data-description) | Original minimum-description-length formulation linking a model and the length of a data description. |
| [Delétang et al., Language Modeling Is Compression, 2023 / ICLR 2024](https://arxiv.org/abs/2309.10668) | Prediction and compression as connected problems; useful context for the essay without asserting that these ReGAE experiments implement a lossless compressor. |
| [Dupont et al., COIN, 2021](https://arxiv.org/abs/2103.03123) | Represents an image through quantized network weights; a direct comparison for image-specific model storage, with coordinate-conditioned reconstruction unlike the Gencoder. |


# Drawing the ReGAE figures

In draft 0.7, reaching the end of an arrow still left a reader with questions. Did the decoder recompute the code on each pass? Did the skip connect two tensors directly, or did something happen before the addition? The architecture figures in 0.8 are organized around answering those questions from the drawing.

[Compare the old and revised U figure on one page](../papers/regae/previews/first-u-comparison-v0.8.pdf).

## What informed the revision

| Reference | What we took from it |
| --- | --- |
| [Marshall, Freitas & Jay, *An evidence-based guidance framework for neural network system diagrams* (2025)](https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0318800) | Their reader and author studies address the diagrams that appear in research papers. In particular, we used their advice to show system inputs and outputs, carry an example through where useful, and give visual marks consistent meanings. The paper uses an observed Webb display detail for the source. The talk tracks a schematic star. Neither stands in for a recorded model output. |
| [Bäuerle, van Onzenoodt & Ropinski, *Net2Vis* (2021)](https://doi.org/10.1109/TVCG.2021.3057483) | Their visual grammar separates the topology of a CNN from the way its layers and feature shapes are drawn. Grouping repeated blocks keeps the overview readable; an enlarged block can then show the operations hidden by that group. |
| [Ronneberger, Fischer & Brox, *U-Net* (2015), Fig. 1](https://arxiv.org/pdf/1505.04597#page=2) | Corresponding resolutions line up across the contracting and expanding paths. The feature maps carry their channel counts and the arrows identify operations. ReGAE uses that reading convention with different mathematics: each lateral connection projects with a learned 1 × 1 convolution and **adds** at the receiving stage. The original U-Net concatenates. |
| [Rougier, Droettboom & Bourne, *Ten Simple Rules for Better Figures* (2014), rules 2–3](https://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1003833) | Give each figure a question and design for the medium in which it will be seen. The paper permits exact stage labels and a caption one can reread. A talk can spend a whole slide on one update or one merge. |

## Two ways to read the drawings

**Trace one sample.** The encoder reads an RGB patch once and writes a 256-coordinate code. The decoder sees a provisional image. On every pass the code stays fixed and the image changes. Following the return arrow should bring the reader to the next image state, never back through the encoder. At the higher level, the gencoder reads a spatial window of codes; it does not receive the external patch code supplied to the pixel decoder.

**Trace one skip.** In the first U of the pixel decoder, the 32 × 32 down stage saves 128 channels. Its skip projects those 128 channels to 160 to match the incoming up path, where the two tensors are added. Only then does the up stage return to 128 channels. The numbers by the two stage outputs alone would disguise the reason for that projection.

The drawings follow the [archived v2 notebook](../notebooks/main_v2.ipynb), specifically the classes `Encoder`, `Decoder`, `ConvU` and `ConvBlock`. Tensor dimensions come from those definitions. Shapes are schematic; volume, arrow width and picture area should not be read as parameter counts or bitrate. None of the illustrations constitutes a new reconstruction experiment.

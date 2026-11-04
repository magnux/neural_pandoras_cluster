# ReGAE paper: proposed narrative and writing design

Proposal 0.1 · November 4, 2026 · For discussion, not an agreed outline.

The manuscript remains at v0.3.1. This document prepares a complete rewrite; it does not revise the paper or call for new experiments. First agree on the argument and section roles, then reread the full manuscript against that plan.

## What the writing research contributes

The sources below offer complementary kinds of guidance. The paper-writing resources are mainly expert recommendations. Dahlstrom synthesizes narrative-communication research focused on nonexpert audiences; it does not establish that a particular storytelling formula improves technical papers. The recommendations for ReGAE are our editorial synthesis.

| Source | Useful guidance | Application here |
| --- | --- | --- |
| [Mensh & Kording, *Ten simple rules for structuring papers* (2017)](https://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1005619), especially Rules 1–4 and 7–9 | Establish one central contribution; outline the logical support for it; make figures and sections advance that argument. | Settle the paper's one-sentence message and the job of each section before rewriting paragraphs. |
| [Simon Peyton Jones, *How to write a great research paper*](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/07/How-to-write-a-great-research-paper.pdf), especially slides on the central idea, contributions, evidence, and reader order | Explain the useful idea explicitly and make each announced contribution traceable to support. Give readers intuition before machinery. | Introduce the reconstruction loop early. Distinguish the model construction, its mathematical interpretation, and the case-study observations. |
| [Jennifer Widom, *Tips for Writing Technical Papers*](https://cs.stanford.edu/people/widom/paper-writing.html), introduction, related-work, body, and appendix guidance | Explain the problem, significance, difficulty, approach, and scope. Place comparisons where readers have the knowledge needed to follow them. | Brief NCA/NCAM attribution belongs early; detailed DiffAE and diffusion/flow comparisons follow the method and worked example. |
| [Gopen & Swan, *The Science of Scientific Writing* (1990)](https://courses.ems.psu.edu/styleforstudents/print/c10_p6.html), authorized reprint | Build from familiar context to new information and align emphasis with the intended meaning. | Introduce sample, code, mutable state, and correction operator before asking readers to understand latent composition or the landscape analogy. |
| [Dahlstrom, *Using narratives and storytelling to communicate science with nonexpert audiences* (2014)](https://www.pnas.org/doi/10.1073/pnas.1320645111) | Narratives can aid engagement and understanding, but vivid cases and apparently inevitable outcomes can also skew interpretation. | Use Pandora as a concrete example of the idea. Its visual appeal cannot establish the generality of the model family or the cause of a sampling behavior. |
| [*Should scientists tell stories?*, Nature Methods editorial (2013)](https://www.nature.com/articles/nmeth.2726) | Logical organization can improve communication; findings that qualify a conclusion must survive the organization. | Present an explanatory progression without inventing a discovery chronology or hiding ambiguous behavior. |

These sources do not prescribe one universal section order. Our choice below is designed for a research paper introducing a modeling principle through an existing exploratory implementation.

## The central message

**Proposed thesis:** ReGAE defines a reusable reconstruction process through direct state prediction without timestep conditioning; its denoising interface supports generation, regeneration, and modeling of learned representations.

Here, “same interface” means an architectural pattern, not shared weights across levels. Generation and regeneration describe uses of the operator; the extent of successful recovery is an empirical question. The paper should distinguish this construction from any claim of general convergence or superiority.

The intellectual motivation is the author's constructive response to “memorization”: what must a model actually learn to reconstruct a dataset, and what information is still supplied externally? This motivates the work without requiring the paper to settle whether neural networks can extrapolate, or to establish that existing models cannot memorize.

The hierarchy of ideas is:

- **Motivation:** understanding what learning a dataset entails.
- **Central contribution:** the ReGAE family and its reusable reconstruction process.
- **Development of that contribution:** apply the modeling interface to representations produced by another instance.
- **Concrete case:** Pandora's Cluster, where sample appearance and spatial relationships are distinct parts of the learning problem.
- **Implications:** a bounded discussion of hierarchical representation, compression, and future recursive modeling.

## The general plot

The opening asks what “learning the data” requires. Reconstructing an individual sample is one task; representing relationships among samples is another. We then introduce ReGAE as a reusable way of reconstructing a desired state. An encoder supplies a representation, and a denoiser repeatedly updates the current state. The biological language of NCA gives a name to restarting that process after damage: regeneration.

Once that mechanism is clear, the next question is what it leaves unspecified. A collection of patch codes can reconstruct a large image if their arrangement is supplied, but a reconstruction mechanism alone does not learn that arrangement. This makes composition the natural next development: use another instance of the modeling interface to learn structure in the representations.

Pandora makes this distinction concrete. The case study explains local reconstruction, the latent field, shifted prediction, and recurrent sampling in that order. Existing observations then show the behavior of this implementation, including ambiguity and apparent relocation. The discussion returns to the opening question: which information has been represented, which remains external, and what does this example suggest about further applications of the same principle?

This is an order of explanation. It is not a claim that the archived v0 and v2 notebooks form a clean chronological or controlled experimental comparison. The abstract and introduction disclose the proposed answer early; the body explains and supports it.

## Proposed section design

| Section | Reader's question | Content and intended takeaway |
| --- | --- | --- |
| **Abstract** | What is the idea, and what does this paper establish? | A compact version of the complete argument: the constructive learning question; ReGAE's reusable denoising process; a short definition of regeneration; the optional composition of instances; the Pandora case and its supportable outcome; the bounded significance. Write it after the argument is agreed. Avoid dimensions, checkpoint sizes, and a catalogue of secondary ideas. |
| **1. Introduction — Learning as a reconstruction problem** | Why should I care about this construction? | Open with the practical meaning of learning a dataset. Explain reconstruction and relationships as different requirements. Introduce generation and repair, with a short NCA/NCAM lineage, then state the ReGAE proposal and preview Pandora as one example. End with a small set of precise contributions and their scope. The reader should know the main idea before encountering implementation detail. |
| **2. Regenerative Autoencoders** | What is the reusable mechanism? | Start with an intuitive encoder–state–denoiser example and the loop diagram. Define the latent code and mutable state, direct state prediction, output feedback, and the absence of a timestep input. Explain training with corrupted states, then generation and re-entry after perturbation. Introduce the unconditional variant and distinguish code conditioning from time conditioning. Close with composition: a second instance can model learned representations. Architecture, loss, dimensionality, and hierarchy depth remain configuration choices. |
| **3. Pandora's Cluster: a worked case study** | What does this design look like when applied to a rich dataset? | Organize around five questions: what information is in the image; what the patch model represents; what spatial information remains unspecified; how the latent model and shifted targets address it; what the existing outputs show. Include overlapping sampling only after its purpose is clear. Separate setup and observations within the section. Present retained evidence at its actual strength and connect reported behavior to existing media when supplied. |
| **4. Connections to denoising and generative dynamics** | How should I understand the design relative to familiar models? | Give a focused comparison with denoising autoencoders, NCA/NCAM, DiffAE, and diffusion/flow formulations. Organize by prediction target, conditioning, state update, and represented structure. Use the computational-field figure here. Acknowledge shared ideas and specify the combination being proposed; the figure is an interpretation, not evidence of a new energy function. Keep derivations in an appendix. |
| **5. Discussion — What has been learned?** | What follows from this case, and what remains open? | Return to where information resides: network parameters, sample codes, and learned relationships. Discuss content ambiguity and apparent teleportation as observations plus possible explanations. State the evidence limits together, with short qualifications retained beside claims that need them. Give architectural reuse and compression a concise paragraph each. The goal is to interpret the work without expanding the paper into separate arguments about AGI, efficient coding, or NVD. |
| **6. Conclusion** | What should I remember and be able to reuse? | Restate the reusable reconstruction principle and the role of the case study in a short closing argument. Connect back to the original question. Introduce no new mechanism or claim. |
| **Appendices** | How can I inspect the implementation and supporting reasoning? | Put exact dimensions, schedules, loss definitions, parameter counts, sampler behavior, archive provenance, training traces, and optional mathematical interpretations here. Preserve the distinction between sufficient mathematical conditions and verified properties of the networks. |

The central transition in Section 3 should be: **representing the samples leaves a relationship-modeling problem, and the same family supplies a way to address it.** This connects the general method to the particular image without making two spatial levels the definition of ReGAE.

## The figures should carry the same argument

| Role in the narrative | Proposed visual | Material |
| --- | --- | --- |
| Explain the reusable process | Encoder, fixed code, mutable state, feedback, and re-entry after perturbation | Adapt the existing architecture schematic. Label it as mechanism, not a measured repair sequence. |
| Make the missing information visible | Source image, patch encodings, their spatial arrangement, and the latent modeling window | New explanatory schematic based on the existing implementation; no new experiment. It should show why patch reconstruction alone leaves ordering external. |
| Support the case-study observations | Representative retained reconstruction/sampling frames, with a relocation sequence if available | Existing author-supplied media, identified by run. Keep the planned role conditional until those assets are provided. Do not substitute generated illustrative images for results. |
| Explain the comparison | Matching contour landscapes, time-conditioned bands, and the ReGAE correction map | Preserve the revised computational-field figure as a conceptual aid, with an explicit schematic caption. |

Figure numbering follows the eventual order. The current Figure 2 may move later. Loss curves remain supporting training records unless they directly answer a question in the main argument.

## Claims and support before prose

| Intended claim | Support available or needed | Writing consequence |
| --- | --- | --- |
| ReGAE defines a direct state-prediction interface that can be reapplied, with optional code conditioning and no time input. | Formulation and implementation. | Present as a construction that readers can inspect. |
| Generation and regeneration use the same state-update interface. | Formulation; archived corruption training. The archive does not contain a controlled repair benchmark. | Explain the mechanism directly, while reserving claims about reliable recovery for supported evidence. |
| The interface can be composed over learned representations. | The conditional image model and separately trained latent Gencoder. | Present this particular composition; broader recursive applications remain a direction. |
| The Pandora implementation produces the reported coherent views and apparent relocations. | Author observations; retained media have yet to be incorporated. | Plan an observational account, not a controlled superiority claim. Missing media limits the claim, not our ability to agree on the outline. |

The logical need for relational information is different from empirical evidence that a particular Gencoder learned it successfully. The rewrite should make that distinction through precise wording and figure choice, without repeating the entire limitations list after every paragraph.

## Material that needs a smaller role

- **Diffusion/ODE geometry:** an explanation and comparison after the core model, with optional mathematical details in an appendix.
- **Gravity:** a short implementation description; the early “neural black holes” account can remain a labeled historical note. It should not carry the main argument.
- **Compression:** a brief interpretation of representation and relationship modeling; the broader thesis belongs to the companion essay.
- **Recursive models and meta-learning:** motivation for future composition. The present hierarchy has separately trained parameters and does not demonstrate self-modeling.
- **NVD:** its own paper.
- **Alternative datasets such as CIFAR-10:** at most one concise example of how composition could change with the dataset; no implication of an experiment.

## Decisions for this outline round

The recommended opening is the constructive memorization question, followed promptly by the ReGAE mechanism. A mechanism-first opening about generation and repair is a viable alternative, but would make dataset learning a later motivation.

First agree on the one-sentence thesis, that opening choice, and the section sequence. Next refine the case-study questions and figure roles using the existing evidence. Only then reread the full draft, map its material into the agreed structure, and write the new paper from that outline. The present document proposes a direction; it does not record agreement.

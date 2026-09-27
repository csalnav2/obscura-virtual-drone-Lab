# References and attribution

Verified September 27, 2026 against publisher, author/institutional, or arXiv records.
These sources support the methods and research context. They do not independently
validate OBSCURA or the Virtual Drone Lab, certify physical invisibility, or
establish successful cloaking at every weather setting. Meta^n and HyperSkill
are cited here as 2026 preprints; no peer-reviewed status is asserted.

## Project provenance

Christopher Salnave. *OBSCURA metamaterial observability lab*. Public project:
[GitHub repository](https://github.com/csalnav2/obscura-metamaterial-observability-lab).
The prepared public update is based on commit
`e258ed2c51c3eeadcb2fd7f2ae47feaf3b33ee98`. The drone extension is the author’s
implementation; references below are method sources, not co-authorship or
endorsement. The complete unpublished code remains outside this public preview.

## Method references

### [1] spall1992

James C. Spall (1992). **Multivariate stochastic approximation using a simultaneous perturbation gradient approximation**. *IEEE Transactions on Automatic Control*. [Source](https://doi.org/10.1109/9.119632).

**Relevance:** SPSA gradient estimation from paired simultaneous perturbations. This establishes the optimization method, not convergence or robustness of the drone implementation.

### [2] pendry2006

John B. Pendry; David Schurig; David R. Smith (2006). **Controlling electromagnetic fields**. *Science*. [Source](https://doi.org/10.1126/science.1125907).

**Relevance:** Transformation-optics background for manipulating electromagnetic fields. The reduced drone surrogate is not a reproduction of a full Maxwell/material cloak.

### [3] braunstein1994

Samuel L. Braunstein; Carlton M. Caves (1994). **Statistical distance and the geometry of quantum states**. *Physical Review Letters*. [Source](https://doi.org/10.1103/PhysRevLett.72.3439).

**Relevance:** Quantum statistical distinguishability and parameter-estimation geometry underlying QFI. Low local parameter sensitivity does not imply universal object undetectability.

### [4] narasimhan2002

Srinivasa G. Narasimhan; Shree K. Nayar (2002). **Vision and the Atmosphere**. *International Journal of Computer Vision*. [Source](https://publications.ri.cmu.edu/vision-and-the-atmosphere).

**Relevance:** Atmospheric attenuation and airlight as mechanisms affecting image visibility, including fog and haze. This does not calibrate the simulator’s rain/dust presets or validate its 0–100% stress scale.

### [5] jolliffe2016

Ian T. Jolliffe; Jorge Cadima (2016). **Principal component analysis: a review and recent developments**. *Philosophical Transactions of the Royal Society A*. [Source](https://doi.org/10.1098/rsta.2015.0202).

**Relevance:** PCA, its SVD formulation and variance-preserving dimension reduction. A changing PCA display does not by itself demonstrate agent learning or causal interpretation.

### [6] shoemake1985

Ken Shoemake (1985). **Animating rotation with quaternion curves**. *ACM SIGGRAPH Computer Graphics*. [Source](https://doi.org/10.1145/325334.325242).

**Relevance:** Unit-quaternion rotation interpolation and smooth animation. This supports the display mathematics, not a claim that latent rotors control navigation.

### [7] kim2026metan

Zae Myung Kim; Young-Jun Lee; Seungyeon Jwa; Dongyeop Kang (2026). **Meta^n: Recursive Self-Improvement through Emergent Depth**. *arXiv preprint*. [Source](https://arxiv.org/abs/2608.24735v1).

**Relevance:** Inspiration for layered meta-reasoning through a fixed operator acting on an expanding input. The paper constructs layers and sets depth by convergence; this simulator prescribes four bounded roles. It is an adaptation, not a reproduction of the paper’s emergent-depth system or benchmarks.

### [8] xu2026hyperskill

Ruiyao Xu; Tiankai Yang; Wei-Chieh Huang (2026). **HyperSkill: Self-Evolving LLM Agents via Hypergraph-Structured Skill Memory**. *arXiv preprint*. [Source](https://arxiv.org/abs/2608.16114v1).

**Relevance:** Inspiration for connecting subtasks and reusable skills through trajectory hyperedges. The paper also specifies retrieval and maintenance procedures; citing it does not claim that all those procedures or benchmark results are reproduced here.

### [9] spall1998

James C. Spall (1998). **An Overview of the Simultaneous Perturbation Method for Efficient Optimization**. *Johns Hopkins APL Technical Digest*. [Source](https://www.jhuapl.edu/spsa/PDF-SPSA/Spall_An_Overview.PDF).

**Relevance:** Accessible author-written introduction to SPSA and its two-evaluation gradient approximation. Acceptance tests and final validation can require additional evaluations.

## How to cite these methods in a post

“Methods and inspiration: SPSA [1,9]; transformation optics [2]; quantum Fisher
information [3]; atmospheric visibility [4]; PCA–SVD [5]; quaternion interpolation
[6]; Meta^n [7]; HyperSkill [8].” The numbered entries above resolve those labels.
Describe the last two as “inspired by” unless a separate implementation audit
establishes exact reproduction. Cite your own experiment logs for performance.

Machine-readable entries are provided in [references.bib](references.bib).

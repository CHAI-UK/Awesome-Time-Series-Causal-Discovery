# Awesome-Time-Series-Causal-Discovery [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

> A curated list of research papers, software tools, datasets, and resources for **Time Series Causal Discovery (TSCD)** — the task of recovering causal structure from observational time series data.
 
This repository is a companion guide to our tutorial paper *(coming soon)*. Whether you are new to the field or an active researcher, we hope it serves as a useful entry point and reference. 



## Contents

- [Why Time Series Causal Discovery?](#why-time-series-causal-discovery)
- [Getting Started](#getting-started)
  - [Key Concepts and Terminology](#key-concepts-and-terminology)
  - [Three Ways to Represent Causality in Time Series](#three-ways-to-represent-causality-in-time-series)
  - [Core Assumptions](#core-assumptions)
  - [Textbooks and References of Note](#textbooks-and-references-of-note)
- [Software and Toolkits](#software-and-toolkits)
- [Benchmark Datasets and Simulation Platforms](#benchmark-datasets-and-simulation-platforms)
- [Best Practices: Evaluation and Tuning](#best-practices-evaluation-and-tuning)
  - [Hyperparameter Selection](#hyperparameter-selection)
- [Surveys and Review Papers](#surveys-and-review-papers)
- [Other Related Works](#other-related-works)
- [Contributing](#contributing)
- [Citation](#citation)


---

## Why Time Series Causal Discovery?
 
The modern study of causality began with time series. Granger's foundational work popularised causal discovery — the problem of detecting causation between time series from observational data alone. Since then, the field has evolved across statistics, dynamical systems, and machine learning, giving rise to a rich but fragmented landscape.
 
Why does this matter? Standard predictive models exploit statistical regularities in data, but they cannot distinguish genuine causal mechanisms from spurious correlations. **TSCD addresses this gap.** By recovering the underlying causal graph from temporal observations, it transforms complex multivariate records into structured, interpretable knowledge that supports:
 
- **Intervention planning**: *If I change variable X, what happens to Y?*
- **Scientific understanding**: Identifying genuine drivers in complex systems (climate, brain, gene regulation)
- **Robust prediction**: Models built on causal structure generalise better under distribution shift
- **AI system design**: Causal priors improve the safety, robustness, and interpretability of downstream models

Time series are a unique modality: temporal information is embedded in the structure of the data itself, enabling the use of temporal precedence ("causes precede effects") as a powerful inductive bias that is unavailable in other data types.


## Getting Started
 
### Key Concepts and Terminology
 
| Term | Meaning |
|------|---------|
| **Data Generating Process (DGP)** | The underlying mechanism that produces the observed data. Think of a program or a set of equations that you can use to generate new data |
| **DAG** | Directed Acyclic Graph — the standard representation of causal structure |
| **Intervention / do-operator** | A formal modification to the DGP; `do(x_t)` clamps variable X to value x at time t, removing all incoming edges to X |
| **Summary Graph** | Shows *which* variables cause which others, with no notion of time lag; may contain cycles |
| **Window Graph** | Shows causal effects within a fixed lag window τ; sufficient to characterise a stationary process |
| **Full Time Graph** | Unrolled graph over the entire observation period; most flexible but intractable for long series |
| **Temporal Precedence** | The principle that causes always precede their effects — the "arrow of time" |
| **Causal Sufficiency** | Assumption that there are no unobserved common causes (latent confounders) in the system |
| **Graphical Time Invariance** | Assumption that the causal graph is unchanging across time |
| **Predictive Causality** | Granger's definition: X causes Y if past X helps predict Y beyond Y's own past |
| **Conditional Independence (CI) Test** | A statistical test used to prune spurious edges by conditioning on confounders/mediators |
| **Additive Noise Model (ANM)** | A functional model Y = f(X) + ε; enables causal direction identification under restrictions on f and ε |
| **Window Length (Q)** | A hyperparameter for the number of time lags considered; Q = τ_max − τ_min |
| **SHD** | Structural Hamming Distance — counts edge additions, deletions, and reversals to transform Ĝ into G |
| **SID** | Structural Intervention Distance — counts errors in implied interventional distributions |
 

### Three Ways to Represent Causality in Time Series
 
TSCD methods produce one of three graph types (see Figure 1 of the tutorial paper):
 
| Graph Type | Description | Notes |
|------------|-------------|-------|
| **Full Graph** | All variables at all time steps; most expressive | Rarely tractable for anything but the shortest series |
| **Window Graph** | Variables at t and a lagged window of size Q; captures time delays explicitly | Preferred; sufficient to characterise a stationary process |
| **Summary Graph** | Collapsed view of the network with no time lag information | Quick overview; note that unlike the others, summary graphs may contain cycles |


### Core Assumptions
 
Almost all TSCD methods implicitly rely on some subset of these assumptions. Understanding which apply to your problem is essential before selecting a method.
 
- **Graphical time invariance** — The causal graph does not change over time. Violated by regime shifts, structural breaks, or evolving dynamics.
- **Causal sufficiency** — All relevant causal variables are observed. Violated when hidden confounders exist. This assumption is *untestable* from data alone — its validity rests on expert judgement.
- **Stochastic vs. deterministic dynamics** — Most methods assume stochastic processes, but some methods assume deterministic dynamical systems (eg, cross-mapping). Applying a method designed for one regime to the other leads to failure.
- **Linearity / function class assumptions** — VARLiNGAM and DYNOTEARS assume linear relationships. Violations reduce identifiability and estimation accuracy. It's important to ensure that your chosen method reflects a reasonable function class for your given problem.
- **Non-Gaussianity** — LiNGAM-family methods require non-Gaussian noise for identifiability of causal direction.
> ⚠️ **No free lunch.** Causal inference always requires assumptions. A method that excels on one benchmark may fail on another simply because the DGP violates its core assumptions. The goal of benchmarking is not to find a universal winner, but to map the applicability boundaries of different algorithms.
 

### Textbooks and References of Note
 
- 📖 **[Causality (2nd ed.) — Pearl (2009)](https://bayes.cs.ucla.edu/BOOK-2K/)** — The foundational text; introduces the do-calculus and DAG framework that underpins most modern TSCD.
- 📖 **[Elements of Causal Inference — Peters, Janzing & Schölkopf (2017)](https://mitpress.mit.edu/9780262037310/elements-of-causal-inference/)** — Free PDF. Bridges causal inference and machine learning; introduces additive noise models and identifiability results.
- 📖 **[Causation, Prediction, and Search — Spirtes, Glymour & Scheines (2001)](https://www.cs.cmu.edu/afs/cs.cmu.edu/project/learn-43/lib/photoz/.g/web/.g/scottd/fullbook.pdf)** — Free PDF. Theoretical basis for constraint-based (PC/FCI) methods.
- 📄 **[Granger (Econometrica, 1969)](https://doi.org/10.2307/1912791)** — The paper that started it all.
- 📄 **[Inferring causation from time series in Earth system sciences — Runge et al. (Nature Communications, 2019)](https://doi.org/10.1038/s41467-019-10105-3)** — Accessible motivating overview of TSCD challenges across domains.
- 📄 **[Causal inference for time series — Runge et al. (Nature Reviews Earth & Environment, 2023)](https://doi.org/10.1038/s43017-023-00431-y)** — A recent survey of the TSCD landscape; highly recommended starting point.
- 📄 **[Causal machine learning for healthcare and precision medicine — Sanchez et al. (Royal Society Open Science, 2022)](https://royalsocietypublishing.org/doi/10.1098/rsos.220638)** — Motivates causal ML in high-stakes application settings.


---

## Software and Toolkits
 
| Tool | Language | Key Methods | Notes |
|------|----------|-------------|-------|
| **[Tigramite](https://github.com/jakobrunge/tigramite)** | Python | PCMCI, PCMCI+, LPCMCI, J-PCMCI+, RPCMCI | Gold standard for constraint-based TSCD; rich CI test library (ParCorr, CMIknn, GPDC); excellent visualisation tools |
| **[lingam](https://github.com/cdt15/lingam)** | Python | VARLiNGAM, LiNGAM, DirectLiNGAM | Official implementation of the LiNGAM family |
| **[CausalNex](https://github.com/mckinsey/causalnex)** | Python | DYNOTEARS, NOTEARS | McKinsey open-source; supports structure learning and probabilistic inference |
| **[causal-learn](https://github.com/py-why/causal-learn)** | Python | PC, FCI, GES, LiNGAM, GRaSP | Part of the py-why ecosystem; broad algorithm coverage for both i.i.d. and time series |
| **[CausalFlow](https://github.com/lcastri/causalflow)** | Python | PCMCI, PCMCI+, DYNOTEARS, VARLiNGAM, TCDF, tsFCI | Unified wrapper framework for many TSCD algorithms with consistent I/O |
| **[causal_discovery_for_time_series](https://github.com/ckassaad/causal_discovery_for_time_series)** | Python | PCMCI, tsFCI, VARLiNGAM, TCDF, DYNOTEARS, TiMINO | Benchmarking-oriented; unified interface for fair comparison of methods |
| **[gcastle](https://github.com/huawei-noah/trustworthyAI/tree/master/gcastle)** | Python | NOTEARS, DYNOTEARS, PC, GES, and more | Huawei's broad causal structure learning toolkit |
| **[TETRAD](https://github.com/cmu-phil/tetrad)** | Java / Python (`py-tetrad`) | PC, FCI, FGES, tsFCI, and many more | CMU's comprehensive suite; decades of development; mature and well-tested |
| **[rEDM](https://github.com/SugiharaLab/rEDM)** | R / Python | CCM, Simplex projection, S-map | Official Sugihara Lab implementation of CCM and empirical dynamic modelling |
| **[pywhy-graphs](https://github.com/py-why/pywhy-graphs)** | Python | Graph utilities | Causal graph data structures and algorithms (py-why ecosystem) |
 
---


## Benchmark Datasets and Simulation Platforms
 
Evaluating TSCD requires datasets with known ground-truth causal graphs — these are almost exclusively synthetic or semi-synthetic.
 
| Resource | Type | Description |
|----------|------|-------------|
| **[CauseMe](https://causeme.uv.es/)** | Simulation + Semi-synthetic | Community benchmarking platform for TSCD; diverse challenge datasets with ground truth; submit results for community comparison |
| **[CausalRivers (ICLR, 2025)](https://openreview.net/forum?id=wmV4cIbgl6)** | Real-world | River network hydrology with verified causal ground truth; highlighted in the tutorial paper as an important "in-the-wild" benchmark. [[code]](https://github.com/causalrivers/experiments) |
| **[CausalTime](https://arxiv.org/abs/2310.01753)** | Real-world inspired | Multi-domain benchmark (air quality, medical, traffic) with ground-truth causal graphs derived from domain knowledge |
| **[TimeGraph](https://arxiv.org/abs/2506.01361)** | Simulation | Synthetic benchmark suite with diverse temporal characteristics; designed for robust evaluation across a broad range of DGP settings |
| **[DREAM3 / DREAM4](https://www.synapse.org/Synapse:syn2787209/wiki/)** | Semi-synthetic | Gold-standard gene regulatory network benchmarks; widely used for TSCD evaluation in bioinformatics |
| **[Tigramite simulation tools](https://github.com/jakobrunge/tigramite)** | Simulation | Built-in VAR and nonlinear time series simulators with fully controllable causal structure, noise type, and coupling strength |
| **[Beware of the simulated DAG! — Reisach et al. (NeurIPS, 2021)](https://arxiv.org/abs/2102.13647)** | ⚠️ Methodological warning | Demonstrates that some DAG simulators can be gamed by exploiting marginal variance rather than true causal structure. Essential reading before designing benchmarks. |
 

---
 
## Best Practices: Evaluation and Tuning
 
### Hyperparameter Selection
 
The performance of TSCD methods is rarely plug-and-play. The tutorial paper identifies two hyperparameters that are **fundamental across almost all TSCD tasks**, and whose poor selection is a common source of failure.
 
**Window Length (Q = τ_max − τ_min)**
 
| Setting | Risk |
|---------|------|
| Too small | Slow-propagating effects (e.g., a chemical process with high latency) will be missed entirely |
| Too large | Conditioning sets grow, increasing computation and introducing noise that raises the false discovery rate |
 
Guidance: set Q to match the longest known physical delay. When domain knowledge is unavailable, ACF/PACF plots offer a data-driven guide. Note that PCMCI scales polynomially with d × τ_max.
 
**Sampling Rate (f_S)**
 
The temporal resolution of recorded data has direct consequences for what TSCD can detect:
 
| Scenario | Consequence |
|----------|-------------|
| Modest downsampling | May *improve* detection by smoothing stochastic fluctuations unrelated to the causal mechanism |
| Aggressive downsampling | Compresses true lagged effects into zero-lag (instantaneous) links; correct directionality becomes hard to determine |
| Extreme subsampling | The system becomes causally underdetermined — the true graph cannot be recovered at any algorithm setting |
 
See [Butler et al. (Asilomar, 2025)](https://doi.org/10.1109/IEEECONF67917.2025.11443432) and [Hyttinen et al. (PGM, 2016)](https://proceedings.mlr.press/v52/hyttinen16.html) for formal treatments of the sampling rate problem.

---
 

 
## Surveys and Review Papers

Although this webpage is a companion to our own TSCD tutorial, there are a number of existing tutorials which may be useful to read for more information and other perspectives on TSCD.
 
- 📄 **[Survey and Evaluation of Causal Discovery Methods for Time Series — Assaad, Devijver & Gaussier (JAIR, 2022)](https://jair.org/index.php/jair/article/view/13428)** — Comprehensive empirical comparison with unified evaluation framework; essential reading.
- 📄 **[Causal inference for time series — Runge et al. (Nature Reviews Earth & Environment, 2023)](https://doi.org/10.1038/s43017-023-00431-y)** — Accessible and up-to-date survey; broader than its Earth science focus.
- 📄 **[Inferring causation from time series in Earth system sciences — Runge et al. (Nature Communications, 2019)](https://doi.org/10.1038/s41467-019-10105-3)** — Motivating case for TSCD in complex observational systems.
- 📄 **[A Survey on Causal Discovery Methods for I.I.D. and Time Series Data — Hasan, Hossain and Gani (2023)](https://arxiv.org/abs/2303.15027)** — Covers both settings with a useful taxonomic overview.


---
 
## Other Related Works

Here are some links to other projects which bear some connection or relationship to this work.
 
- **Causal Discovery for i.i.d. Data** — TSCD methods exploit temporal structure unavailable in cross-sectional data. For i.i.d. settings, see [causal-learn](https://github.com/py-why/causal-learn) and the [Awesome Causal Inference](https://github.com/matthewvowels1/Awesome-Causal-Inference) list.
- **Causal Effect Estimation** — Distinct from causal discovery. Once a graph is known, [DoWhy](https://github.com/py-why/dowhy) and [EconML](https://github.com/microsoft/EconML) can estimate the magnitude of causal effects and support counterfactual reasoning.
- **[py-why ecosystem](https://www.pywhy.org/)** — Growing collection of Python causal tools working toward a unified causal ML stack (DoWhy, causal-learn, EconML, pywhy-graphs).
- **Spatiotemporal Causal Discovery** — See [CausalST_Papers](https://github.com/yutong-xia/CausalST_Papers) for causality extended to spatial dimensions.
- **Signal Processing Considerations** — [Butler et al. (Asilomar, 2025)](https://doi.org/10.1109/IEEECONF67917.2025.11443432) discusses underappreciated signal processing factors (sampling rate, aliasing, window length selection) in TSCD from a signal processing perspective.

---

## Contributing
 
Contributions are very welcome! Please submit a **pull request** if you have:
 
- A paper, tool, dataset, or resource that fits the taxonomy
- A correction or broken link fix
  
**Format for paper entries:**
```
📄 **[Title — Authors (Venue, Year)](URL)** — One-sentence description. [[code]](code_url)
```
 
**Format for tool entries:** Add a row to the Software table with name, language, key methods, and a brief note.
 
Please ensure links are working and entries are placed in the appropriate section. If unsure where something fits, open an issue for discussion.
 
---

## Citation
Please cite our paper if you found our tutorial paper helpful:
```bibtex
Coming soon
```

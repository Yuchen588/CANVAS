# CANVAS
[![R >4.0](https://img.shields.io/badge/R-%3E%3D4.0-brightgreen)](https://www.r-project.org/) ![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.6809147.svg) ![jupyter](https://img.shields.io/badge/Jupyter--notebook-EV_SpaTalk--tutorial-yellow?logo=jupyter) ![DOI](https://img.shields.io/badge/DOI-10.1038%2Fs41467--022--32111--8-yellowgreen)

### Cellular Architecture and Neighborhood-informed Virtual AI-driven Spatial-reconstruction

CANVAS is a biologically grounded, cross-modality AI framework that integrates high-dimensional spatial proteomics with routine histopathology to reconstruct, quantify, and model tumor ecological habitats directly from H&E-stained tissue sections. By learning to transfer CODEX-defined cellular neighborhoods (CNs) onto native histology, CANVAS enables accurate and scalable inference of spatial habitats across whole-slide images. This cross-modality alignment is powered by vision–language foundation modeling, refined through AI-agent–guided biological interpretation, and optimized via efficient spatial feature selection and machine learning–based prognostic modeling. Spanning single-cell resolution to population-scale clinical inference, CANVAS builds a functional bridge between spatial biology and precision oncology, enabling interpretable and translatable spatial biomarker discovery.
________________________________________
## Key Modules
# (1) CN-to-habitat prediction via a vision–language foundation model
`
CANVAS employs a foundation model trained on paired CODEX–H&E whole-slide data to infer CN-defined ecological habitats directly from unannotated histology. This module integrates:
>	Visual context derived from high-resolution H&E morphology, capturing both cellular composition and tissue architecture
>	(Optional) Language-driven semantic priors, conceptually designed to encode CN-specific cell-type composition, functional states, and immunologic roles. Although not used in the current implementation, this component is modular and supports prompt-based alignment for future extensions
>	Contrastive supervision between CODEX-defined CNs and histology-inferred habitats to enforce spatial and biological correspondence
`
Run: do_CANVAS.py
Output: Patch-level probability maps of predicted habitat classes across entire whole-slide H&E images, enabling virtual spatial annotation with cellular-scale resolution
________________________________________
# (2) Habitat-level spatial feature generation
For each inferred habitat, CANVAS extracts a suite of biologically interpretable spatial features spanning six core domains:
•	Composition: Cell-type abundance and immune–stromal partitioning
•	Diversity: Shannon index, Fisher alpha, richness, and entropy-based metrics
•	Spatial metrics: Ripley’s K/L/F functions, Clark–Evans index, and kernel density estimates
•	Interaction: Cell–cell proximity networks, triplet motifs, barrier and clustering scores
•	Distance: Intercellular distance matrices stratified by immunologic lineage
•	Transition entropy: Metrics of spatial heterogeneity and boundary complexity
Run: do_feature_generation.R
Output: Multiscale spatial feature matrices per sample
________________________________________
(3) Feature selection and prognostic modeling
To identify clinically meaningful spatial biomarkers, CANVAS performs:
•	Bootstrap LASSO and RandomForestSRC to assess feature stability and importance
•	Univariate Cox regression to generate interpretable survival associations
•	Multivariate modeling to build predictive signatures across training and validation cohorts
•	Subtype stratification across histological and molecular classes (e.g., LUAD vs. LUSC, EGFR/KRAS mutant subsets)
Run: do_feature_modeling.R
Output: Refined spatial biomarker panels and prognostic models optimized for immunotherapy response and clinical outcome prediction
________________________________________
(4) AI-agent guidance and spatial explainability
CANVAS integrates an AI-agent module to trace each spatial feature back to its cellular and architectural origin, enhancing interpretability across scales. Specifically, the agent:
•	Maps predictive features (e.g., STE, Ripley’s K, proximity scores) to cell-type composition and local spatial topology
•	Dissects spatial arrangements such as immune–stromal segregation, lymphoid clustering, or fibrotic encapsulation
•	Highlights spatial ecosystems associated with known mechanisms of immunotherapy resistance (e.g., T cell exclusion, TLS presence, suppressive myeloid hubs)
Run: do_AI_agent.py
Output: A biologically coherent explanation layer linking high-level model outputs to underlying spatial architectures in tissue
________________________________________
Applications
•	Immunotherapy response stratification in NSCLC and other solid tumors
•	Habitat-informed prognostic modeling across large-scale cohorts (TCGA, PLCO, NLST)
•	Molecular subtyping independent of PD-L1 status or canonical driver mutations
•	Virtual spatial proteomic annotation inferred directly from standard H&E histology
________________________________________
Citation
Li et al., Interpretable AI reconstructs histologic spatial ecosystems to advance precision oncology in lung cancer (2025)


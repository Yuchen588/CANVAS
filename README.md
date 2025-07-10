# CANVAS

<table>
<tr>
<td width="60%" valign="top">

### A cross-modality AI framework for ecological habitat reconstruction from histopathology

**CANVAS** (***C**ellular **A**rchitecture and **N**eighborhood-informed **V**irtual **A**I-driven **S**patial-reconstruction*) is a biologically grounded, cross-modality AI framework that integrates high-dimensional spatial proteomics with routine histopathology to reconstruct, quantify, and model tumor ecological habitats directly from H&E-stained tissue sections. By learning to transfer CODEX-defined cellular neighborhoods (CNs) onto native histology, CANVAS enables accurate and scalable inference of spatial habitats across whole-slide images. This cross-modality alignment is powered by vision–language foundation modeling, refined through AI-agent–guided biological interpretation, and optimized via efficient spatial feature selection and machine learning–based prognostic modeling. Spanning single-cell resolution to population-scale clinical inference, CANVAS builds a functional bridge between spatial biology and precision oncology, enabling interpretable and translatable spatial biomarker discovery.

</td>
<td width="60%" align="center" valign="middle">

<img src="https://github.com/Yuchen588/CANVAS/blob/main/CANVAS_image.png?raw=true" style="max-width:280px; height:auto;">

</td>
</tr>
</table>

### 🔧 Key Modules

#### (1) CN-to-habitat prediction via a vision–language foundation model

CANVAS employs a foundation model trained on paired CODEX–H&E whole-slide data to infer CN-defined ecological habitats directly from unannotated histology. This module integrates:

- Visual context derived from high-resolution H&E morphology
- *(Optional)* Language-driven semantic priors encoding CN-specific composition, functions, and immune roles *(modular but not used in current version)*
- Contrastive supervision across CODEX–histology habitat pairs to ensure spatial and biological correspondence
- Cross-modality co-registration at single-cell resolution to anchor CODEX-defined cellular neighborhoods within histologic context

**Run:**

```bash
python do_CANVAS.py
python do_registration.py
```

**Output:** Patch-level probability maps of predicted habitat classes across whole-slide H&E images.

#### (2) Habitat-level spatial feature generation

For each inferred habitat, CANVAS extracts a suite of biologically interpretable spatial features spanning six domains:

- **Composition:** Relative abundance of components and immune–stromal partitioning
- **Diversity:** Habitat heterogeneity quantified by Shannon index, Fisher alpha, richness, and entropy
- **Spatial metrics:** Aggregation and dispersion captured via Ripley’s K/L/F functions, Clark–Evans index, and kernel density
- **Interaction:** Frequency of spatial co-occurrence or avoidance based on proximity-defined networks
- **Distance:** Minimum intra-habitat distances between key functional elements
- **Transition entropy:** Non-Euclidean metrics reflecting internal heterogeneity and boundary complexity

**Run:**

```bash
Rscript do_feature_generation.R
```

**Output:** Multiscale spatial feature matrices per sample.

#### (3) Feature selection and prognostic modeling

To identify clinically meaningful spatial biomarkers, CANVAS performs:

- Bootstrap LASSO and RandomForestSRC for feature stability and importance
- Univariate Cox regression for interpretable survival association
- Multivariate modeling across cohorts for outcome prediction
- Subtype stratification across histologic and mutational subclasses (e.g., LUAD vs LUSC, EGFR/KRAS)

**Run:**

```bash
Rscript do_feature_selection.R
Rscript do_feature_modeling.R
```

**Output:** Refined biomarker panels and risk prediction models for immunotherapy outcomes.

#### (4) AI-Agent module for spatial feature interpretation

CANVAS integrates a large language model (LLM)–powered AI agent to enhance interpretability by mapping high-dimensional spatial features to their underlying cellular architecture and topological context. 

The agent provides structured explanations across five biological dimensions:

- (i) feature category,
- (ii) associated habitat cellular composition,
- (iii) spatial property description,
- (iv) topological coupling tendency,
- (v) biological and clinical implications.

**Run:**

```bash
python do_AI_agent.py
```

**Output:** A biologically coherent explanation layer linking model-derived features to spatial architecture.

---

### 🚀 Applications

- Immunotherapy response stratification in NSCLC and other solid tumors
- Habitat-informed prognosis modeling across TCGA, PLCO, and NLST
- Molecular subtyping independent of PD-L1 status or canonical mutations
- Virtual spatial proteomic annotation inferred from routine histology

---

### 📦 Installation

We recommend using `conda` to manage environments for CANVAS.

```bash
conda create -n canvas_env python=3.8
conda activate canvas_env
pip install -r requirements.txt
```

R packages used include:

```r
install.packages(c("survival", "glmnet", "randomForestSRC", "ggplot2", "vegan", "entropy", "spatstat"))
```

---

### 📂 Directory Structure

```
CANVAS/
├── demo_data/ # Example data including input image and spatial feature matrices
│ ├── H&E_image/ # Raw H&E histology image
│ ├── Image_habitat_prediction.csv # Predicted habitat labels for each image patch
│ └── Spatial_feature_matrix.csv # Spatial feature matrix for each sample
│
├── habitat_prediction/ # Module 1: Habitat prediction from H&E using vision models
│ ├── habitat_prediction.py # Performs CN-to-habitat inference via foundation model
│ └── co-registration.py # Optional: CODEX and histology cell-cell alignment
│
├── spatial_feature/ # Module 2: Spatial feature extraction
│ └── spatial_feature_extra.R # Calculates composition, diversity, interaction, and more
│
├── modeling/ # Module 3: Feature selection and prognostic modeling
│ ├── feature_select_model.R # Bootstrap LASSO and RandomForestSRC for stable feature ranking
│ └── prog_model.R # Cox regression and multivariate modeling for risk prediction
│
├── AI_Agent/ # Module 4: AI-agent for biological interpretation
│ └── AI_agent.py # Traces key features back to underlying spatial architecture
│
├── README.md # This file
```

### 📄 Citation

Li Y. et al. *Cellular Architecture and Neighborhood-informed Virtual Spatial Tumor Profiling from Histopathology* (2025)

---

### 📬 Contact

Project maintained by the Department of Radiation Oncology, Stanford University.\
📧 [ycli16@stanford.edu](mailto\:ycli16@stanford.edu)

---

### 🧠 Acknowledgements

We thank the developers of CODEX, Seurat, PyTorch, and foundational libraries.\
This work was supported by NIH, NSF, and institutional funds from Stanford University.


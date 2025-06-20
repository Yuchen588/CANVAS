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

**Run:**

```bash
python do_CANVAS.py
```

**Output:** Patch-level probability maps of predicted habitat classes across whole-slide H&E images.

#### (2) Habitat-level spatial feature generation

For each inferred habitat, CANVAS extracts a suite of biologically interpretable spatial features spanning six domains:

- **Composition:** Cell-type abundance, immune–stromal partitioning
- **Diversity:** Shannon index, Fisher alpha, richness, entropy
- **Spatial metrics:** Ripley’s K/L/F functions, Clark–Evans index, kernel density
- **Interaction:** Cell–cell proximity networks, motif embeddings, barrier scores
- **Distance:** Intercellular distance matrices stratified by immune lineage
- **Transition entropy:** Non-Euclidean metrics of spatial heterogeneity and boundary complexity

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
Rscript do_feature_modeling.R
```

**Output:** Refined biomarker panels and risk prediction models for immunotherapy outcomes.

#### (4) AI-agent guidance and spatial explainability

CANVAS incorporates an AI-agent module to enhance interpretability by linking spatial features to their cellular and topological origins. Specifically, the agent:

- Maps high-level features (e.g., STE, Ripley’s K) to cellular composition and topology
- Highlights immune–stromal segregation, fibrotic barriers, and lymphoid clustering
- Prioritizes spatial configurations associated with immune exclusion and therapy resistance

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
├── figures/                 # Schematic and output images
├── demo_data/              # Sample inputs (H&E image, CN maps)
├── model/                  # Pretrained models (vision foundation models)
├── scripts/                # Python and R scripts
├── outputs/                # Example outputs: maps, features, models
├── requirements.txt        # Python dependencies
├── README.md               # This file
```

---

### 🧪 Usage Example

```bash
python do_CANVAS.py        # habitat prediction
Rscript do_feature_generation.R   # spatial feature extraction
Rscript do_feature_modeling.R     # prognostic model training
python do_AI_agent.py      # spatial feature interpretation
```

---

### 📄 Citation

Li Y. et al. *Interpretable AI reconstructs histologic spatial ecosystems to advance precision oncology in lung cancer* (2025)

---

### 📬 Contact

Project maintained by the Department of Radiation Oncology, Stanford University.\
📧 [ycli16@stanford.edu](mailto\:ycli16@stanford.edu)

---

### 🧠 Acknowledgements

We thank the developers of CODEX, Seurat, PyTorch, and foundational libraries.\
This work was supported by NIH, NSF, and institutional funds from Stanford University.


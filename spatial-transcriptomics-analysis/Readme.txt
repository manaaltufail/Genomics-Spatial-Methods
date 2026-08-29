 Spatial Transcriptomics Analysis — Scanpy & Squidpy

A collection of four Jupyter notebooks covering spatial transcriptomics analysis using **Scanpy** and **Squidpy**, progressing from foundational spot-level clustering to sub-cellular resolution analysis. Each notebook works on **Google Colab** using publicly available 10x Genomics datasets.

---

## Repository Structure

Recommended layout (flatten the current nested folders into this):

```
spatial-transcriptomics-analysis/
├── 01_scanpy_basic/
│   └── basic_analysis.ipynb          # Core Scanpy spatial workflow
├── 02_visium_fluo/
│   └── analyze_visium_fluorescence.ipynb   # Image segmentation + feature extraction
├── 03_visium_hne/
│   └── squidpy_visium_hne.ipynb      # Spatial statistics & cell-cell communication
├── 04_xenium/
│   └── squidpy_xenium.ipynb          # Sub-cellular resolution Xenium analysis
└── README.md
```

---

## Notebooks at a Glance

 [01 — Basic Scanpy Spatial](./01_scanpy_basic/)
**Dataset:** Human Lymph Node (Visium)

The foundational spatial workflow: QC filtering, normalization, highly-variable gene detection, PCA, UMAP, Leiden clustering, and visualization of clusters and covariates (`total_counts`, `n_genes_by_counts`) overlaid on the H&E tissue image.

---

 [02 — Visium Fluorescence Image Analysis](./02_visium_fluo/)
Dataset:** Visium fluorescence tissue crop

Watershed segmentation of the fluorescence image, extraction of segmentation/texture/histogram image features per spot, and Leiden clustering on those image-derived features — compared against transcriptomic clusters.

---

 [03 — Visium H&E Spatial Statistics](./03_visium_hne/)
Dataset:** Mouse Brain, coronal section (Visium + H&E)

Multi-scale image feature extraction and morphology-based clustering, neighborhood enrichment (which cluster pairs co-localize beyond chance), distance-dependent co-occurrence analysis, ligand–receptor interaction analysis (CellPhoneDB-style via OmniPath), and Moran's I to identify spatially variable genes.

---

 [04 — Xenium Single-Cell Spatial](./04_xenium/)
Dataset:** Human Lung (Xenium V1, ~161,000 cells × 480 genes)

True single-cell resolution analysis loaded via `spatialdata-io`. Standard QC → normalize → PCA → Leiden → UMAP pipeline, Delaunay-based spatial neighbor graph, centrality scores (closeness, clustering coefficient, degree) per cluster, and co-occurrence/Moran's I on a subsampled set for efficiency.

---

 Technology Stack

| Tool | Role |
|---|---|
| [Scanpy](https://scanpy.readthedocs.io/) | Core single-cell analysis (QC, normalization, clustering, UMAP) |
| [Squidpy](https://squidpy.readthedocs.io/) | Spatial statistics, graph construction, image features |
| [spatialdata-io](https://spatialdata.scverse.org/) | Xenium data loading (notebook 4) |
| [AnnData](https://anndata.readthedocs.io/) | Core data structure for expression + spatial metadata |
| Leiden algorithm | Graph-based community detection |
| UMAP | Non-linear dimensionality reduction |

---

## Installation

```bash
 Notebooks 1–3
pip install scanpy squidpy anndata leidenalg

 Notebook 4 (additional)
pip install spatialdata-io
```

All notebooks download their datasets automatically — either via `scanpy.datasets`, `squidpy.datasets`, or directly from the 10x Genomics public server — so they're ready to run on Google Colab or locally with no manual data setup.

> The Xenium dataset (~3 GB) can take several minutes to download and may require a runtime restart after installing dependencies — see the in-notebook instructions.

---

## Key Biological Concepts

- Visium vs. Xenium:** Visium captures 55 µm multi-cell spots with H&E or fluorescence imaging; Xenium achieves true single-cell (sub-cellular) resolution via in situ transcript detection.
- patial graphs:** Connect spots/cells by proximity to enable neighborhood-based statistics.
- Neighborhood enrichment:** Permutation test for spatial co-localization of cluster pairs.
- Co-occurrence:** Distance-dependent probability of two clusters being found near each other.
- Moran's I:** Spatial autocorrelation statistic — measures whether gene expression is spatially clustered, random, or dispersed.
- Centrality metrics:** Graph-theoretic measures (closeness, degree, clustering coefficient) of how "hub-like" a cluster is within the tissue.

---

 References
- [Squidpy documentation & tutorials](https://squidpy.readthedocs.io/en/stable/tutorials.html)
- [Scanpy documentation](https://scanpy.readthedocs.io/)
- [10x Genomics Visium](https://www.10xgenomics.com/spatial-transcriptomics/)
- [10x Genomics Xenium](https://www.10xgenomics.com/platforms/xenium)


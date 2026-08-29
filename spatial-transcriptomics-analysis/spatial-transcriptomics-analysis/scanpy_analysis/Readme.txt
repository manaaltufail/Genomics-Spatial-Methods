01 — Basic Scanpy Spatial Analysis

Dataset: 10x Genomics Visium, Human Lymph Node
Source: downloaded automatically via `scanpy.datasets.visium_sge`

 What this notebook does

The foundational spatial transcriptomics workflow using Scanpy on 10x Visium data (based on the Scanpy tutorial by Giovanni Palla). It also notes how the same approach extends to MERFISH data.

1. Reading the data** — loads the Visium human lymph node dataset into an `AnnData` object containing counts, spatial coordinates, and the H&E tissue image.
2. QC and preprocessing** — filters spots by total counts and number of expressed genes, normalizes counts (`normalize_total`), and detects highly-variable genes.
3. Manifold embedding and clustering** — PCA → neighbor graph → UMAP → Leiden clustering based on transcriptional similarity.
4. Visualization in spatial coordinates** — overlays `total_counts`, `n_genes_by_counts`, and cluster assignments as circular spots on top of the H&E image using `scanpy.pl.spatial`, including cropping and image transparency options.

 Key functions used
- `sc.datasets.visium_sge`
- `sc.pp.normalize_total`, `sc.pp.highly_variable_genes`
- `sc.pp.pca`, `sc.pp.neighbors`, `sc.tl.umap`, `sc.tl.leiden`
- `sc.pl.spatial`

How to run
bash
pip install scanpy

Open the notebook and run all cells top to bottom — the dataset downloads automatically on first run.

 Notes
- No internet-free/offline data is bundled here; the dataset is pulled from 10x Genomics servers at runtime.
- For up-to-date tutorials on this same workflow, see the [Squidpy tutorials](https://squidpy.readthedocs.io/en/stable/tutorials.html).

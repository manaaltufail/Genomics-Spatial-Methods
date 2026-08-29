 03 — Visium H&E Spatial Statistics

Dataset: 10x Genomics Visium, Mouse Brain (coronal section, H&E stained)
Source: Squidpy's built-in Visium H&E dataset (pre-processed `AnnData` + `ImageContainer`)

 What this notebook does

Demonstrates Squidpy's spatial-statistics toolkit on a Visium H&E dataset, relating tissue morphology, gene expression clusters, and spatial organization.

1. Load data — pre-processed `AnnData` (expression matrix + cluster annotations) and the high-resolution H&E image, loaded together.
2. Visualize cluster annotations — plots existing cluster labels in spatial context to get a first look at tissue architecture.
3. Extract image features — computes summary statistics from the H&E image at two scales (1.0 and 2.0) to capture both fine local texture and broader tissue context.
4. Cluster by image morphology— scales the extracted features, runs PCA, builds a neighbor graph, and computes a new Leiden clustering based purely on image morphology (independent of gene expression) for comparison.
5. Neighborhood enrichment** — permutation test on the spatial connectivity graph identifying which cluster pairs are spatially co-located more or less than expected by chance.
6. Co-occurrence across distances — measures the probability of observing one cluster near another as a function of distance radius (conditioned on the Hippocampus cluster in this example).
7. Ligand–receptor interaction analysis — runs `sq.gr.ligrec()` (a fast CellPhoneDB-style method using the OmniPath database) with 100 permutations, testing all annotated ligand–receptor pairs across cluster combinations, and visualizes significant Hippocampus interactions.
8. Spatially variable genes (Moran's I) — computes Moran's I on the top 1,000 highly variable genes to find genes with non-random spatial expression patterns, then plots the top hits on the tissue.

 Key functions used
- sq.im.calculate_image_features
- sq.gr.spatial_neighbors, sq.gr.nhood_enrichment
- sq.gr.co_occurrence
- sq.gr.ligrec
- sq.gr.spatial_autocorr (Moran's I)
- sq.pl.spatial_scatter, sq.pl.nhood_enrichment, sq.pl.co_occurrence, sq.pl.ligrec

 How to run
bash
pip install scanpy squidpy leidenalg

Run all cells in order. If running on Google Colab, install dependencies in the first cell as instructed.

 Notes
- Moran's I is restricted to the top 1,000 HVGs for computational efficiency — increase this if you need a more exhaustive scan.
- Ligand-receptor analysis depends on the OmniPath database, which requires an internet connection at runtime.

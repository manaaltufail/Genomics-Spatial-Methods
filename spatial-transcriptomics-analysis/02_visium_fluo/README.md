# 02 — Visium Fluorescence Image Analysis

**Dataset:** Visium fluorescence tissue crop
**Source:** `squidpy.datasets.visium_fluo_image_crop()` and `squidpy.datasets.visium_fluo_adata_crop()`

## What this notebook does

Analyzes Visium fluorescence data by combining gene expression with image-derived morphological features.

1. **Load data** — pulls the pre-processed fluorescence image (`img`) and matching `AnnData` (`adata`) from Squidpy's built-in datasets.
2. **Initial visualization** — plots existing gene-expression clusters (`sq.pl.spatial_scatter`) and inspects the raw fluorescence channels (`img.show(channelwise=True)`).
3. **Image processing & segmentation** — smooths the image (`sq.im.process`) and segments it with the **watershed algorithm** (`sq.im.segment`), then visualizes the segmentation result side-by-side with the raw image.
4. **Feature extraction** — computes segmentation, texture, and histogram image features per spot at multiple scales (`sq.im.calculate_image_features`), with several parameter configurations (`features_orig`, `features_context`, etc.).
5. **Feature-based clustering** — a helper function (`cluster_features`) scales the extracted features, runs PCA, builds a neighbor graph, and computes Leiden clusters independently for the summary, histogram, and texture feature sets.
6. **Comparison** — plots the image-morphology-based clusters spatially, alongside the transcriptomic clusters, to see how well tissue structure visible in the image aligns with gene-expression clusters.

## Key functions used
- `sq.datasets.visium_fluo_image_crop`, `sq.datasets.visium_fluo_adata_crop`
- `sq.im.process`, `sq.im.segment`, `sq.im.calculate_image_features`
- `sq.pl.spatial_scatter`, `sq.pl.extract`
- `sc.pp.scale`, `sc.pp.pca`, `sc.pp.neighbors`, `sc.tl.leiden`

## How to run
```bash
pip install anndata scanpy squidpy leidenalg
```
Open the notebook and run all cells in order — the image/AnnData crop downloads automatically.

## Notes
- Segmentation uses `channel=0`; adjust if your own fluorescence data has the nuclear/DAPI channel elsewhere.
- `sq.im.calculate_image_features` is run with `n_jobs=1` here for reproducibility; increase this for faster runs on larger images.

# 04 — Xenium Single-Cell Spatial Analysis

**Dataset:** 10x Genomics Xenium V1, Human Lung, 2-FOV (sub-cellular resolution, ~161,000 cells × 480 genes)
**Source:** downloaded directly from the 10x Genomics public server (`https://cf.10xgenomics.com`)

## Pipeline

| Step | Task |
|---|---|
| 1 | Install packages — no version pins, pip resolves automatically |
| 2 | Download + unzip the Xenium dataset (~3 GB) into `./Xenium/` |
| 3 | Load with `spatialdata-io` |
| 4 | QC, filtering, normalization, clustering |
| 5 | Spatial neighbor graph + spatial statistics |

## What this notebook does

1. **Install packages** (~3–4 min) — restart the runtime when done, if on Colab.
2. **Import libraries** — full analysis stack; warnings suppressed for clean output.
3. **Download the dataset** — pulls Xenium V1 Human Lung 2-FOV and extracts it locally; skips re-downloading if the folder already exists.
4. **Load data in-memory** — uses `spatialdata_io.xenium()` with images disabled and never calls `sdata.write()`. This avoids a known issue where the large multi-scale morphology TIFFs (20,000 × 51,000 px) are loaded lazily by dask with irregular chunk shapes that Zarr rejects on write.
5. **Extract the AnnData table** — gene expression lives in `sdata.tables` as an `AnnData` object: `adata.X` (counts), `adata.obs` (per-cell metadata: transcript counts, cell/nucleus area), `adata.obsm["spatial"]` (x, y coordinates). Note the table key can vary between `spatialdata-io` versions.
6. **QC metrics** — `sc.pp.calculate_qc_metrics` adds `total_counts` and `n_genes_by_counts`; negative control probe/codeword counts are checked to confirm low background (<1%).
7. **QC histograms** — inspects total transcripts, unique genes, cell area, and nucleus ratio to choose filtering thresholds.
8. **Filter cells and genes** — conservative filters: cells with ≥10 total counts, genes expressed in ≥5 cells.
9. **Normalize, reduce, cluster** — saves raw counts to a layer, `normalize_total`, `log1p`, PCA (50 PCs), neighbors graph, Leiden clustering.
10. **UMAP visualization** — colored by total counts, gene count, and Leiden cluster.
11. **Spatial scatter** — projects Leiden clusters onto real tissue (x, y) coordinates to reveal tissue regions (airways, stroma, immune infiltrate, etc.).
12. **Spatial neighborhood graph** — Delaunay triangulation (`coord_type="generic"`) for free-form cell coordinates.
13. **Centrality scores** — closeness centrality, clustering coefficient, and degree centrality per cluster, describing how "hub-like" each cluster is spatially.
14. **Subsampling for expensive analyses** — co-occurrence and Moran's I are O(n²) in cell count, so 50% of cells are subsampled for just these two steps; everything else uses the full dataset.

## Key functions used
- `spatialdata_io.xenium`
- `sc.pp.calculate_qc_metrics`, `sc.pp.filter_cells`, `sc.pp.filter_genes`
- `sc.pp.normalize_total`, `sc.pp.log1p`, `sc.pp.pca`, `sc.pp.neighbors`, `sc.tl.leiden`, `sc.tl.umap`
- `sq.gr.spatial_neighbors`, `sq.gr.centrality_scores`, `sq.gr.co_occurrence`, `sq.gr.spatial_autocorr`

## How to run
```bash
pip install scanpy squidpy spatialdata-io leidenalg
```
Run cells in order. Expect the download + install steps to take several minutes, and a runtime restart may be required after installing dependencies (Colab).

## Notes
- Do **not** call `sdata.write()` on this dataset with images enabled — it will fail due to irregular dask chunk shapes in the morphology TIFFs.
- Because of the large cell count, co-occurrence and Moran's I are run on a 50% subsample; adjust the subsample fraction if you have more compute available.

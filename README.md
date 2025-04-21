# ATAC-Seq Chromatin Accessibility Analysis with ArchR

This repository contains an end-to-end pipeline to analyze chromatin accessibility profiles from single-nucleus ATAC-seq (snATAC-seq) datasets using **ArchR**. The pipeline includes dimensionality reduction, clustering, marker identification, and visualization across over 10 processed brain samples.

## Project Overview
Chromatin accessibility is a powerful proxy for identifying active regulatory regions across the genome. This project processes over 100,000 single-nucleus profiles to characterize cell-type-specific accessibility in mouse brain samples, supporting downstream discovery of cell identity and state.

## Objectives
- Load and manage snATAC-seq data using ArchR arrow files
- Perform iterative LSI for dimensionality reduction
- Cluster cells and generate UMAP embeddings
- Identify major brain cell types based on gene activity scores
- Assess data quality through fragment length periodicity and TSS enrichment

## Dataset Description
The input comprises **12 Arrow files** generated from 10X Genomics Multiome mouse brain samples.

| Sample Count | Source | Genome |
|--------------|--------|--------|
| 12           | 10X Genomics Multiome | mm10 (mouse) |

ArchR auto-generates:
- TileMatrix
- GeneScoreMatrix

Total Cells: ~100,000

## Pipeline Summary

### 1. Load and Prepare Project
- Load or initialize `ArchRProject` with Arrow files
- Assign genome reference (`mm10`)

### 2. Dimensionality Reduction
- Run `addIterativeLSI()` using TileMatrix
- Generate reduced dimensions (LSI1 to LSI30)

### 3. Clustering & Visualization
- Graph-based clustering with `addClusters()`
- UMAP visualization with `addUMAP()`
- Generate cluster-wise and gene-wise embeddings

### 4. Cell Type Annotation
- Impute gene scores with MAGIC
- Identify major cell types using marker genes:
  - Neurons: *Rbfox3, Slc17a7, Gad1*
  - Oligodendrocytes: *Mog, Mbp*
  - Astrocytes, Microglia, OPCs, Endothelial cells, etc.
- Validate annotations with browser track visualizations

### 5. Quality Control
- **Fragment Length Periodicity**: Validates nucleosomal structure
- **TSS Enrichment**: Reflects accessibility around transcription start sites

## Key Results
- **Clusters Identified**: 13 major clusters
- **Cell Types Resolved**: Excitatory and inhibitory neurons, oligodendrocytes, astrocytes, microglia, OPCs
- **TSS Enrichment**: Confirms high-quality snATAC-seq data across most samples
- **Fragment Periodicity**: Visible mono-, di-, and tri-nucleosome peaks

## Scalability
This ArchR-based pipeline is adaptable for larger or heterogeneous datasets:
- Compatible with custom reference genomes
- Modular steps for integration with multi-omics (RNA+ATAC)
- Processes ~100k cells in under 10 minutes on HPC nodes
- Easily re-used for human, cancer, or drug-response datasets with minimal changes

## Tech Stack
- **R (ArchR)**
- Data visualization with `ggplot2`, `grid`, and ArchR's built-ins
- Genome annotation via `mm10`

## Visualizations
### 1. Cluster Identification via Iterative LSI  
This UMAP plot shows the result of dimensionality reduction using Iterative Latent Semantic Indexing (LSI), followed by clustering of cells based on chromatin accessibility profiles. Each cluster is color-coded and reveals distinct subpopulations across mouse brain samples, providing insights into underlying cell-type diversity.  
<img width="270" alt="Screenshot 2025-04-20 at 9 33 28 PM" src="https://github.com/user-attachments/assets/449daac4-bfe5-4a86-9af6-ccb8844dfd70" />

### 2. Marker Gene Accessibility Map  
UMAP projection of the same cells, colored by gene accessibility scores for **RBFOX3**, a neuron-specific marker. The gradient illustrates differential accessibility across clusters, helping to annotate cell identities and confirm known lineage-specific expression patterns.  
<img width="319" alt="Screenshot 2025-04-20 at 9 33 51 PM" src="https://github.com/user-attachments/assets/24ff20b1-6657-4065-ae01-805c2ed73761" />

### 3. Chromatin Accessibility Across Loci  
Track plot showing normalized ATAC-seq signal across the **Gad1/Gad1os** locus, separated by cluster. This figure captures region-specific regulatory activity and highlights which cell types show enriched chromatin accessibility at functionally relevant genomic regions.  
<img width="393" alt="Screenshot 2025-04-20 at 9 35 37 PM" src="https://github.com/user-attachments/assets/eaa7e450-1a25-44bd-b03e-f77e8a1f4f4a" />


## Applications
- Cell-type deconvolution in complex tissues
- Comparative epigenomics across conditions
- Foundation for multi-omics integration with scRNA-seq

## License
This project is distributed under the MIT License.

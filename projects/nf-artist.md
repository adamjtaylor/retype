---
icon: image
---

# nf-artist: Multiplexed Tissue Imaging Visualization Pipeline

## About

nf-artist is a Nextflow pipeline developed to automate the generation of interactive visualizations from high-dimensional tissue imaging data. Originally built for the Human Tumor Atlas Network (HTAN) Data Portal, this tool transforms complex microscopy datasets into accessible, web-based explorations that enable researchers to investigate tumor microenvironments without specialized software.

Multiplexed tissue imaging generates massive, multi-channel datasets that are difficult to visualize and share. Researchers working with technologies like Cyclic Immunofluorescence (CyCIF) produce images with 40+ protein markers per tissue section, stored in specialized formats (SVS, CZI, OME-TIFF) that require expert software to view. This creates barriers to data sharing, collaboration, and reproducibility in cancer research.

The pipeline addresses this by orchestrating four key processing stages:

- **Format Standardization**: Automated conversion of proprietary bioformats to OME-TIFF standard, preserving full image pyramid structure for multi-resolution access

- **Interactive Story Generation**: Integration with the Minerva ecosystem to create self-contained HTML visualizations with automatic channel thresholding and tiled image pyramids for efficient web streaming

- **Thumbnail Generation**: Dimensionality reduction (UMAP/t-SNE/PCA) to compress 40+ channels into intuitive RGB thumbnails with configurable colormaps optimized for perceptual uniformity

- **Batch Processing at Scale**: CSV-driven sample sheet input for processing hundreds of images with parallel execution and configurable resource allocation

The pipeline supports deployment on local Docker environments, AWS Batch for production workloads, and Nextflow Tower for interactive monitoring. All dependencies are version-pinned in multi-stage Docker builds, ensuring reproducibility across environments and over time.

## My Role

I designed and implemented nf-artist from the ground up, making key architectural decisions around modularity, reproducibility, and scalability. My contributions included:

- Designing the modular subworkflow architecture that separates conversion, visualization, and thumbnail generation into independent, testable components

- Implementing conditional processing logic that intelligently routes images through appropriate pipelines based on metadata flags, handling both multiplexed fluorescence and H&E brightfield images through a unified interface

- Building the CI/CD infrastructure with three GitHub Actions workflows: automated Docker builds with semantic versioning, integration testing against representative datasets, and weekly security scanning with Trivy

- Configuring AWS Batch deployment profiles with graduated retry logic and resource scaling for fault-tolerant cloud execution

- Integrating four external specialized tools (Miniature, Auto-Minerva, Minerva-Author, bioformats2raw) into a cohesive pipeline with version-pinned dependencies

The pipeline now supports the HTAN consortium's mission to make tumor atlas data publicly accessible, reducing the time from data acquisition to shareable visualization from days to hours.

## Resources

- [GitHub Repository](https://github.com/Sage-Bionetworks-Workflows/nf-artist)
- [Container Image](https://ghcr.io/sage-bionetworks-workflows/nf-artist)
- [HTAN Data Portal](https://humantumoratlas.org/)

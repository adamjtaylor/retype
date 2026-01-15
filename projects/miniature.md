---
icon: paintbrush
---

# Miniature: Visualizing High-Dimensional Tissue Imaging Data

## About

Highly multiplexed tissue imaging methods have transformed biomedical research by enabling the localization of 10-100 proteins at sub-micron resolution across centimeter-scale tissue sections. Technologies including CyCIF, CODEX, MIBI, IMC, and Immuno-SABER are now central to large-scale atlas programs like the Human Tumor Atlas Network (HTAN) and HuBMAP. However, this high dimensionality creates a fundamental visualization challenge: how do you generate an informative image preview that captures the full heterogeneity of a 40+ channel image?

Traditional approaches using multi-channel overlays are limited to 4-6 channels before colors become indistinguishable, discarding the vast majority of captured information. Data portals presenting thousands of imaging datasets need thumbnails that enable researchers to quickly recognize tissue features, assess heterogeneity, and recall previously viewed samples.

Miniature solves this by treating each pixel as a point in high-dimensional space and using unsupervised dimensionality reduction to project the full channel information into 2D or 3D coordinates that can be mapped to perceptually meaningful colors. The result: a single thumbnail image where similar colors indicate similar protein expression profiles, revealing tissue architecture and pathological features without requiring any prior knowledge of marker identity.

## Scientific Approach

The Miniature pipeline operates on OME-TIFF pyramidal images through four stages:

**1. Pyramid Selection & Background Removal**
Low-resolution pyramid layers are extracted to enable fast processing. Otsu thresholding automatically segments tissue from background, focusing computation on biologically relevant pixels.

**2. Dimensionality Reduction**
Three methods are supported:
- **UMAP**: Preserves local neighborhood structure, revealing tissue heterogeneity
- **t-SNE**: Strong local structure with different global characteristics
- **PCA**: Fast linear projection for rapid previews

**3. Perceptually Optimized Color Encoding**
For 3D embeddings, I implemented the U-CIE algorithm in Python, which optimizes the fit of the embedding point cloud into the sRGB portion of L\*a\*b\* color space through rotation, translation, and scaling. This maximizes perceptual distinguishability while ensuring all colors are displayable. For 2D embeddings, six bivariate colormaps (BREMM, SCHUMANN, STEIGER, TEULING2, ZIEGLER, CUBEDIAGONAL) provide alternatives optimized for different perceptual tasks.

**4. Quality Assessment**
Trustworthiness metrics based on Venna & Kaski (2001) quantify how well the thumbnail preserves neighborhood relationships from the original high-dimensional data. I extended this with perceptual trustworthiness using CIEDE2000 color difference, measuring whether perceptually similar colors truly reflect similar protein expression profiles.

## Validation & Results

Systematic parameter evaluation across 92 combinations of dimensionality reduction method, scaling, and colormap revealed that UMAP to 3D with UCIE color encoding provides the optimal balance of perceptual trustworthiness and visual appeal. Critically, perceptually distinct regions in Miniature thumbnails correspond to:

- Known pathological features visible in H&E staining of the same section
- Leiden clusters generated from cell segmentation and unsupervised clustering
- Tissue structures including tumor regions, immune infiltrates, and stromal compartments

Color vision deficiency simulations confirmed that thumbnails remain interpretable across deuteranopia, protanopia, tritanopia, and other CVD variants—ensuring accessibility for the estimated 8% of males affected by color vision deficiency.

## My Role

I am the sole author and developer of Miniature. Key contributions include:

- **Algorithm Design**: Developed the complete pipeline from concept through implementation, including a Python re-implementation of the U-CIE color optimization algorithm with ConvexHull-based constraint testing achieving 5-20x speedups over the original approach

- **Perceptual Metrics**: Designed novel trustworthiness metrics combining embedding fidelity with CIEDE2000 perceptual color difference, enabling principled parameter optimization

- **Accessibility Analysis**: Conducted systematic color vision deficiency simulations to ensure inclusive design

- **Production Deployment**: Built CLI tools, Docker containers, and a Nextflow pipeline (nf-artist) for scalable batch processing on AWS

- **Scientific Communication**: Authored the methodology paper describing the approach, validation, and applications

## Impact

Miniature thumbnails are now deployed on the **Human Tumor Atlas Network Data Portal** (data.humantumoratlas.org), providing visual previews for over 2,700 multiplexed tissue imaging datasets across eight imaging modalities. The tool enables researchers to:

- Rapidly browse and triage imaging datasets without downloading full-resolution files
- Recognize tissue architecture and identify samples with specific features of interest
- Recall and re-identify previously viewed datasets based on visual characteristics

The approach has broad applicability beyond HTAN, with potential applications in other data portals (HuBMAP, BioImage Archive), as outputs from imaging pipelines (MCMICRO), and as rendering states in visualization tools (Minerva, Napari).

## Resources

- **Publication**: Taylor AJ. "Miniature: Unsupervised glimpses into multiplexed tissue imaging datasets as thumbnails for data portals." *bioRxiv* (2024). [doi:10.1101/2024.10.01.615855](https://doi.org/10.1101/2024.10.01.615855)
- **GitHub Repository**: [github.com/adamjtaylor/miniature](https://github.com/adamjtaylor/miniature)
- **HTAN Data Portal**: [data.humantumoratlas.org](https://data.humantumoratlas.org)

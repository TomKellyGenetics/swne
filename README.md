# SWNE
Similarity Weighted Nonnegative Embedding (SWNE), is a method for visualizing high dimensional datasets.
SWNE uses Nonnegative Matrix Factorization to decompose datasets into latent factors, projects
those factors onto 2 dimensions, and embeds samples and key features in 2 dimensions relative to the
factors. SWNE can capture both the local and global dataset structure, and allows
factors and relevant features to be embedded directly onto the visualization, facilitating interpretation
of the data.

You can find our bioRxiv preprint [here](https://www.biorxiv.org/content/early/2018/06/22/276261).

## Installation
Run the following code to install the package using devtools:

```
if(!require(devtools)){
install.packages("devtools") # If not already installed
}
devtools::install_github("yanwu2014/swne")
```

## Quickstart
Download the example [Seurat object](https://bit.ly/2B3Q3LN)

```
## Load data
obj <- readRDS("pbmc3k_seurat.Robj")

## Get clusters
clusters <- obj@ident; names(clusters) <- obj@cell.names;

## Build SNN
obj <- BuildSNN(obj, dims.use = 1:20, k.param = 20, prune.SNN = 1/20)

## Run SWNE
genes.embed <- c("MS4A1", "GNLY", "CD3E", "CD14",
                 "FCER1A", "FCGR3A", "LYZ", "PPBP", "CD8A")
swne.embedding <- RunSWNE(obj, k = 10, genes.embed = genes.embed)

## Plot SWNE
PlotSWNE(swne.embedding, alpha.plot = 0.4, sample.groups = clusters,
         do.label = T, label.size = 3.5, pt.size = 1.5, show.legend = F,
         seed = 42)
```

![](Examples/seurat_quickstart_swne.png?raw=True "SWNE plot of 3k PBMCs")


## Walkthroughs and examples
Since SWNE is primarily meant for visualization and interpretation of the data, we typically use either [Seurat](http://satijalab.org/seurat/) or [Pagoda2](https://github.com/hms-dbmi/pagoda2) as a primary scRNA-seq pipeline. All the R markdown files used to generate the walkthroughs can be found under the Examples/ directory.

1. A basic [walkthrough](https://yanwu2014.github.io/swne/Examples/pbmc3k_swne_seurat.html) of 3k PBMC cells starting from a pre-computed Seurat object.
2. A basic [walkthrough](https://yanwu2014.github.io/swne/Examples/pbmc3k_swne_pagoda2.html) of 3k PBMC cells starting from a pre-computed Pagoda2 object.
3. A [walkthrough](https://yanwu2014.github.io/swne/Examples/multiple_pancreas_alignment_swne.html) using SWNE to visualize four pancreas datasets that have undergone batch alignment with Seurat's [manifold alignment](https://www.nature.com/articles/nbt.4096). The script used to generate the object can be found [here](https://yanwu2014.github.io/swne/Examples/multiple_pancreas_workflow.R) and the raw datasets can be found [here](http://bit.ly/IAexpmat).
4. A [walkthrough](https://yanwu2014.github.io/swne/Examples/cortical_neuron_projection.html) using SWNE to project new data onto an existing SWNE embedding. In this case, we're projecting a neuronal dataset generated using the C1 system onto a neuronal dataset generated using snDropSeq.


## Recreating Figures
To recreate the figures from our preprint, see the Scripts/ directory. 

To generate the simulated discrete and trajectory datasets, use `splatter_generate.R`. The simulated datasets we generated can be found [here](https://bit.ly/2JQDDNc)

To generate the visualizations and embedding evaluations, run `splatter_discrete_swne.R` and `splatter_trajectory_swne.R` for the discrete and trajectory simulations, respectively. To benchmark SWNE runtimes, use `splatter_runtime_analysis.R`.

The data needed to run `hemato_swne.R` can be found [here](https://bit.ly/2MFiByO) and the raw data for the hematopoietic cells can be found, courtesy of the monocle2 developers, [here](http://www.gs.washington.edu/~xqiu/proj2/RGE_analysis_data.tar.gz).

The data needed to run `snDropSeq_swne.R` on the cerebellar and visual cortex data can be found [here](https://bit.ly/2I6R5XL) and the raw data can be found at the GEO accession GSE97930.

The raw PBMC dataset can be found at the 10X genomics [website](https://support.10xgenomics.com/single-cell-gene-expression/datasets/1.1.0/pbmc3k).

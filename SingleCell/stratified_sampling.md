# Stratified Sampling of a Seurat Object

This tutorial will teach you how to perform stratified sampling on a Seurat object. This is useful if you're dealing with a large Seurat object that you'd like to 
run some preliminary analysis on. 

### Required Packages
```r
if(!require("tidyverse")) install.packages("tidyverse")
if(!require("Seurat")) install.packages("Seurat")
```

### Create a data frame of your cells and clusters 
Using Seurat's ```FetchData``` function, you'll first want to fetch the cells and their associated clusters. If you have given your clusters an annotation
name, you'll put that name as a parameter to the function. Otherwise, you'll use the default ```seurat_clusters``` name.

```r
cells <- as_tibble(FetchData(seurat.obj, vars = c("Seurat_Assignment")), rownames = "Cells")
```

### Filter for the clusters you want and sample 

We'll now filter for each of the clusters we are interested in sampling. 

```r
cluster.sample_1 <- cells %>% filter(Seurat_Assignment == "Cluster_1") %>% slice_sample(n = 200, replace = FALSE) %>% pull(Cells) 
cluster.sample_2 <- cells %>% filter(Seurat_Assignment == "Cluster_2") %>% slice_sample(n = 200, replace = FALSE) %>% pull(Cells) 
...
```
We can now take these cells and subset our original Seurat object.

```r
seurat.obj.subset <- subset(seurat.obj, cells = c(cluster.sample_1, cluster_sample_2) 

```



## Creating a Custom SingleR Reference Based Upon a Seurat Object

A SingleR reference is composed of the following: 
- A matrix with cell IDs and gene names
- Celltype labels for each cell ID

This is a fairly straight forward process with an annotated Seurat object. First, extract the counts using `GetAssayData()`:

`ref <- GetAssayData(seurat_obj, assay = "RNA")` <br>

Then, extract the annotated clusters. The exact code will differ, depending on how you stored the cluster names. If you stored them as a new type of meta-data, extract them through: 

`labels <- seurat_obj@meta.data$Cluster_Names`

If you simply renamed the identities like guided by the Seurat PBMC tutorial, then extract them by:

`labels <- seurat_obj@meta.data$ident`

Now, when you run SingleR, just set ref = ref, and labels = labels, and the rest of the parameters depending on what exactly you are wanting: 

`SingleR(test = single_cell_obj, ref = counts, labels = labels, method = "single")`

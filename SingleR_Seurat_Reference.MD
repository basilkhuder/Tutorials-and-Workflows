## Creating a Custom SingleR Reference Based Upon a Seurat Object

A SingleR reference is composed of the following: 
- A matrix with gene names as rows and cell IDs as columns
- Labels for each sample

The biggest issue with using a single-cell dataset is that *samples* are individual cells, which eat up computational resources if we 
try making it into a SingleR reference. To circumvent this, we'll use SingleR's *aggregateReference* 
function that will aggregate cells into a pseudo-bulk sample. For more about the downsides/assumptions to this approach, refer
to the [SingleR manual.](https://bioconductor.org/packages/devel/bioc/vignettes/SingleR/inst/doc/SingleR.html#6_reference_options)

With that in mind, the first thing we'll do is extract the counts using Seurat's `GetAssayData()`:

`ref <- GetAssayData(seurat_obj, assay = "RNA")` <br>

Then, extract the annotated clusters. The exact code will differ, depending on how you stored the cluster names. If you stored them as a new type of meta-data, extract them through: 

`labels <- seurat_obj@meta.data$Metadata_Name`

If you simply renamed the identities like guided by the Seurat PBMC tutorial, then extract them by:

`labels <- seurat_obj@meta.data$ident`

Finally, we'll use SingleR's *aggregateReference* for pseudo-bulk aggregation:

`ref <- aggregateReference(ref,labels)`

Now, when you run SingleR, just set ref = ref, and labels = ref$labels, and the rest of the parameters depending on what exactly you are wanting: 

`SingleR(test = single_cell_obj, ref = ref, labels = labels, method = "single")`

### Using Seurat's AverageExpression Function
We can also use Seurat's *AverageExpression* function to aggregate our cells into a pseudo-bulk. Just like before, we'll extract counts using *GetAssayData* and labels through our object's meta-data. But, we'll use *AverageExpression* instead of SingleR's *aggregateReference*:

```
seurat_obj_avg <- AverageExpression(seurat_obj, assay = "RNA")
labels <- colnames(seurat_obj_avg)
SingleR(test = single_cell_obj, ref = seurat_obj_ag, labels = labels, method = "single")
```

## Custom SingleR Reference for Cell-Type Overrepresentation

If you're dealing with a Seurat object composed of multiple samples, it can be useful to analyze them against each other to pinpoint cell-type overrepresentation. 
One method is run SingleR with each individual sample against a custom reference built from the subset. So for example, if our Seurat 
object $$a$$ is a set containing samples $$a$,$$b$,$$c$,$$d$$

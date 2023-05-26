###This file contains the code I used for Generating Heatmaps in R Studio
# Date of Work: 18 May


## Option 1: Simple Heatmap ## 

#Upload gene.matrix 
`gene.matrix <- read.table("Google Drive/My Drive/Documents /SHH_Temrinal Files/out.panarootest_gff_190/gene_presence_absence.Rtab",header = T, stringsAsFactors = F, check.names = T, comment.char = "", quote = "", row.names = 1)`

View(structure.matrix)
# Convert gene matrix to presence/absence matrix
`presence_matrix <- ifelse(gene.matrix != 0, 1, 0)`

# Create a heatmap
`heatmap(presence_matrix, Rowv = NA, Colv = NA, col = colorRampPalette(c("white", "blue"))(100),
scale = "none", xlab = "Serogroups", ylab = "Genes",
main = "Distribution of Genes between Serogroups")`


## Option 2: Heatmap with Phylogenetic Clustering## 

## Upload the gene.matrix in R from the gene_prescense_absence.Rtab file
`gene.matrix <- read.table("Google Drive/My Drive/Documents /SHH_Temrinal Files/out.panarootest_gff_190/gene_presence_absence.Rtab",
header = T, stringsAsFactors = F, check.names = T, comment.char = "", quote = "", row.names = 1)
class(gene.matrix$SRO100_LPS)`

# change to numeric
`gene.matrix <- gene.matrix %>%
 mutate_if(is.integer, ~as.numeric(as.integer(.)))
class(gene.matrix$SRO100_LPS)`

# convert to matrix
`gene.matrix <- as.matrix(gene.matrix)
pangenome.hm <- heatmap(gene.matrix, Rowv=NA, Colv=NA, col = c("white","black"))
pangenome.hm`

#create a heatmap 
`pangenome.hm2 <- heatmap(gene.matrix, col = c("white","black"))
pangenome.hm2`


## Option 3: Advanced Heatmap with Phylogenetic Clustering ## 
# load packages  
`library(reshape2)`

# convert data into "long" format ("melt")
`gene.matrix.m <- melt(gene.matrix)`

# set colours for presence/absence
`colr.pres <- c("1" = "black", "0" = "white")
pangenome.hm <- ggplot(gene.matrix.m, aes(Var1, Var2, fill = value)) +
 geom_tile() +`

#scale_alpha_identity(guide = "none") + 
coord_equal(expand = 0) +
theme_bw(base_size = 8) +
theme(panel.grid.major = element_blank(),
       axis.title = element_blank(),
       legend.position = "none") +
scale_fill_gradient(low = "white", high = "black")
Pangenome.hm


API
===

.. autosummary::
   :toctree: generated

   testing

# About
*tk

## What is Projectionist?

Projectionist is a tool for **visualizing human gut microbiome samples** by integrating them with information learned from the Human Microbiome Compendium. Samples (and any relevant metadata fields) are *tk

Projectionist enables biologists to evaluate their data from a new perspective by integrating novel 16S data with tens of thousands of publicly available sequencing data.

>:books:**Citing Projectionist**
>
> authors *tk, "Projectionist: PCA contextualization of microbiome data with international compendium." bioRxiv [Preprint]. *tk https://doi.org/*tk

## What is a "projection"?

*tk similar to performing PCA by combining samples with compendium data, but user data does not factor into the calculation of eigenvalues.

## Can I use a custom ordination?</h3>

**Yes.** Projectionist can use any loadings that define axes as linear combinations of (transformed) taxonomic read counts. That is, Projectionist can apply loadings that are formatted in this way:

| kingdom  | phylum | class | order | family | PC1 | PC2 | PC3 | \[...\] | PC*n* |
| ------------- | ------------- | ------------- | ------------- | ------------- | ------------- | ------------- | ------------- | ------------- | ------------- |
| Bacteria  | Actinomycetota  | Coriobacteriia  | Coriobacteriales  | Atopobiaceae  | 0.400  | 0.001  | -1.01  | ... | 0.03  |
| Bacteria  | Bacillota  | Bacilli  | Lactobacillales  | Streptococcaceae  | 1.410  | -1.301  | -5.00  | ... | 0.01  |
| ...  |   |   |   |   |   |   |   |  |   |
| Bacteria  | Bacillota  | Clostridia  | Christensenellales  | Christensenellaceae  | 0.000  | 2.333 | 0.359  | ... | 0.000  |

## How should I format my input?

*tk explanation of using DADA2 for processing.

### Taxonomic table

One file is needed to perform the projection: a taxonomic table in which each column is a taxon, each row is a sample, and each cell contains the **untransformed read counts** indicating how many reads in a given sample were classified with a given taxon.

[!NOTE]
User data is passed through robust centered log-ratio transformation, which will return identical results for read counts or percentages. However, **each input sample is first rarefied to 3000 reads** before projection, to avoid introducing bias caused by differences in library size. This means Projectionist requires **a table of read counts**, rather than proportions.

#### Column names
"sample"

### Metadata table
*tk

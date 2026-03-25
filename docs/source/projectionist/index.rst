===============
Projectionist
===============


.. toctree::
  reference

About
======
tk

For technical details of the package and web app, see :doc:`reference` for tk

Frequently Asked Questions
============================


.. contents::
  :local:

What is Projectionist?
^^^^^^^^^^^^^^^^^^^^^^^^^^

Projectionist is a tool for **visualizing human gut microbiome samples** by integrating them with information learned from the Human Microbiome Compendium. Samples (and any relevant metadata fields) are tk

Projectionist enables biologists to evaluate their data from a new perspective by integrating novel 16S data with tens of thousands of publicly available sequencing data.

**Citing Projectionist**
|
| authors tk, "Projectionist: PCA contextualization of microbiome data with international compendium." bioRxiv [Preprint]. tk https://doi.org/tk
|

What is a "projection"?
^^^^^^^^^^^^^^^^^^^^^^^^^^

tk similar to performing PCA by combining samples with compendium data, but user data does not factor into the calculation of eigenvalues.

Can I use a custom ordination?
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Yes.** Projectionist can use any loadings that define axes as linear combinations of (transformed) taxonomic read counts. That is, Projectionist can apply loadings that are formatted in this way:

========    ============== ============== ================== =================== ===== ====== ===== ===== =====
kingdom     phylum         class          order              family              PC1   PC2    PC3   [...] PC *n*
========    ============== ============== ================== =================== ===== ====== ===== ===== =====
Bacteria    Actinomycetota Coriobacteriia Coriobacteriales   Atopobiaceae        0.400 0.001  -1.01 ...   0.03
Bacteria    Bacillota      Bacilli        Lactobacillales    Streptococcaceae    1.410 -1.301 -5.00 ...   0.01
...
Bacteria    Bacillota      Clostridia     Christensenellales Christensenellaceae 0.000 2.333  0.359 ...   0.000
========    ============== ============== ================== =================== ===== ====== ===== ===== =====

User data will be loaded as a samples x taxon table of read counts, which we then rarefy to 3000 reads per sample see https://github.com/blekhmanlab/compendium_website/issues/34):

====== ===== ===== ===== ===== ===== ==========
Sample Tax 1 Tax 2 Tax 3 Tax 4 Tax 5 READ TOTAL
====== ===== ===== ===== ===== ===== ==========
s123   300   150   600   650   1300  3000
s124   250   1000  1700  50    0     3000
s125   0     0     1400  1300  300   3000
====== ===== ===== ===== ===== ===== ==========

At that point, we need to perform the `robust centered log-ratio transformation`_. This is how they define it:

.. math::
  rclr = log\frac{x}{g(x > 0)}

The `g` here is a value for each sample indicating the geometric mean of the read counts found in all taxa. For `s123` above would be

.. math::
  \sqrt[5]{(300 \times 150 \times 600 \times 650 \times 1300)} = 469.5092

This is where the controversial step comes in. Zeroes are a problem here, for both the geometric mean (which uses multiplication) and the logarithms. There are a few options to replace zeroes with other numbers, but the rCLR transformation we're going with **skips the zeroes**:
* The geometric mean calculated for `s125` above would be `(1400*1300*300)^(1/3)`, for example.
* For the subsequent steps using logs, the zeroes in the matrix (s124, taxon 5; s125, taxa 1 and 2) would **remain unchanged**.


.. _robust centered log-ratio transformation: https://journals.asm.org/doi/10.1128/msystems.00016-19

Once each sample has its geometric mean, that number is used to "center" the data in its row by dividing each read count by the geometric mean, then taking the log.

.. code-block:: r

    gm_mean = function(x){
      exp(mean(log(x[x > 0])))
    }

    rclr <- function(a) {
      answer <- log(a/gm_mean(a))
      answer[] <- lapply(answer, function(i) if(is.numeric(i)) ifelse(is.infinite(i), 0, i) else i)
      return(answer)
    }

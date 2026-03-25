================================
Usage Reference
================================

.. contents::

Web application
================

tktk


R utilities
================

Data formatting
==================

tk explanation of using DADA2 for processing.

Taxonomic table
-----------------

One file is needed to perform the projection: a taxonomic table in which each column is a taxon, each row is a sample, and each cell contains the **untransformed read counts** indicating how many reads in a given sample were classified with a given taxon.

.. important::
   Projectionist requires **a table of read counts**, rather than proportions. **Each input sample is first rarefied to 3000 reads** before projection, to avoid introducing bias caused by differences in library size.

Column names
---------------
"sample"

Metadata table
---------------
taking

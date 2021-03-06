==========================================
Notes on papers
==========================================
:date: 2014-10-01 00:00
:summary: Short notes on read papers
:start_date: 2014-10-01
:end_date: 2015-10-01

Othologous Gene Clusters and Taxon Signature Genes for Viruses of Prokaryotes (2013)
====================================================================================
Summary

- Phage Orthologous Groups (POGs)
- 1,000 genomes, including genomes of dsDNA (88%), ssDNA (10%), ssRNA and dsRNA
  (2%). Also archael viruses (still calling it POG though). 93% of the dsDNA
  belonged to tailed phages of the order Caudovirales.
- *Orthologous groups* Edge-Search algorithm. Only E values of <10 and covering
  at least 50% of the protein lengths. Protein belonging to multiple groups is
  always an error (only 1% of all proteins).
- 57 taxa to find signature genes for. Only using those with at least 3
  distinct viruses and removing temporary collections or unclassified viruses.
  Used 100% precision (only correct genomes), then highest recall (found in most
  genomes).
- host listing in GenBank of archaeal and bacterial species
- COG-building method on 97,731 proteins (or domains) from 1,027 virus genomes
  were clustered into 4,542 POGs
- POG size from a minimum of 3 proteins from 3 distinct viruses up to 673
  proteins from 378 virus. Most of the POGs are small with with a median size
  of 5 proteins from 5 viruses
- no POG found in more than 37% of the 1,027 virus genomes

Personal Notes
- *Bacillus* phage G, largest known phage genome - Two methods of viral
  reproduction: lysogenic cycle and lytic cycle - Caudoviralis (caudo means
  tail, order of viruses). No sequence similarity for DNA or amino acids of
  families in that order, just morphology.
- `Inovirus <http://viralzone.expasy.org/all_by_species/558.html>` (nos means
  muscle in Greek). ssVirus

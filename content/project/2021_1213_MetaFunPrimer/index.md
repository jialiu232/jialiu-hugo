---
author: Jia Liu
categories:
- Python
- Bash
date: "2021-12-13"
draft: false
excerpt: MetaFunPrimer designs high-throughput qPCR primers for environmental abundant microbiome functional genes. 
featured: true
layout: single
links:
- icon: book-open
  icon_pack: fas
  name: doi
  url: https://doi.org/10.1128/mSystems.00201-21
- icon: github
  icon_pack: fab
  name: code
  url: https://github.com/pommevilla/MetaFunPrimer.git
subtitle: ""
tags:
- hugo-site
title: MetaFunPrimer
---

*— [Chris House, A Complete Guide to CSS Grid Layout](http://chris.house/blog/a-complete-guide-css-grid-layout/)* [^1]

---
Genes belonging to the same functional group may include numerous
and variable gene sequences, making characterizing and quantifying difficult.
Therefore, high-throughput design tools are needed to simultaneously create primers for improved quantification of target genes. We developed MetaFunPrimer, a
bioinformatic pipeline, to design primers for numerous genes of interest. This tool
also enables gene target prioritization based on ranking the presence of genes in
user-defined references, such as environment-specific metagenomes. Given inputs of
protein and nucleotide sequences for gene targets of interest and an accompanying
set of reference metagenomes or genomes, MetaFunPrimer generates primers for
ranked genes of interest. To demonstrate the usage and benefits of MetaFunPrimer,
a total of 78 primer pairs were designed to target observed ammonia monooxygenase subunit A (amoA) genes of ammonia-oxidizing bacteria (AOB) in 1,550 publicly
available soil metagenomes. We demonstrate computationally that these amoA-AOB
primers can cover 94% of the amoA-AOB genes observed in the 1,550 soil metagenomes compared with a 49% estimated coverage by previously published primers.
Finally, we verified the utility of these primer sets in incubation experiments that
used long-term nitrogen fertilized or unfertilized soils. High-throughput quantitative
PCR (qPCR) results and statistical analyses showed significant differences in relative
quantification patterns between the two soils, and subsequent absolute quantifications also confirmed that target genes enumerated by six selected primer pairs were
significantly more abundant in the nitrogen-fertilized soils. This new tool gives microbial ecologists a new approach to assess functional gene abundance and related
microbial community dynamics quickly and affordably

[^1]: The original article cited here is now updated and maintained by the staff over at CSS-Tricks. Bookmark their version if you want to dive in and learn about CSS Grid: [A Complete Guide to Grid](https://css-tricks.com/snippets/css/complete-guide-grid/)

---
title: "Knudson's data"
author: "Daniel Piqué <dpique1@gmail.com>"
output: 
  html_document:
    keep_md: yes
---

## Revisiting Knudson's 2 Hit Hypothesis: A Lesson in Data Reproducibility

Knudson's ["Mutation and cancer: statistical study of retinoblastoma"](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC389051/) (1971) forms the basis for the 2-hit hypothesis in cancer and has 8000+ citations in the biomedical literature.

The goal of this vignette is to reproduce [Figure 1](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC389051/?page=4) from this paper using the data provided in [Table 1](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC389051/?page=2), which is also provided within this package as the object `k`. 


## Loading the `knudson` library

`knudson` is a data package that contains a single object, `k`, which is a machine-readable form of Table 1 from Knudson, 1971. 

We'll also load some other packages that we'll need for the rest of the analysis. 





``` r
library(knudson)
library(tidyverse)
library(patchwork)
library(png)
library(knitr)
library(kableExtra)
```

## Looking at Table 1

Let's load a screenshot of Table 1 taken from the original article. We'll also peek at a machine readable form of the Table 1, `k`.


![plot of chunk unnamed-chunk-2](figure/unnamed-chunk-2-1.png)


``` r
# `k` is a machine-readable version of Table 1 
kable(knudson::k) %>%
  kable_styling(position="center") %>%
  scroll_box(width = "100%", height = "200px")
```

<div style="border: 1px solid #ddd; padding: 0px; overflow-y: scroll; height:200px; overflow-x: scroll; width:100%; "><table class="table" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;"> case </th>
   <th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;"> hosp_numb </th>
   <th style="text-align:left;position: sticky; top:0; background-color: #FFFFFF;"> sex </th>
   <th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;"> age_at_dx </th>
   <th style="text-align:left;position: sticky; top:0; background-color: #FFFFFF;"> r_tum </th>
   <th style="text-align:left;position: sticky; top:0; background-color: #FFFFFF;"> l_tum </th>
   <th style="text-align:left;position: sticky; top:0; background-color: #FFFFFF;"> family_hx </th>
   <th style="text-align:left;position: sticky; top:0; background-color: #FFFFFF;"> side </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 3712 </td>
   <td style="text-align:left;"> F </td>
   <td style="text-align:right;"> 8 </td>
   <td style="text-align:left;"> * </td>
   <td style="text-align:left;"> * </td>
   <td style="text-align:left;"> no </td>
   <td style="text-align:left;"> bilat </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 11571 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:left;"> * </td>
   <td style="text-align:left;"> * </td>
   <td style="text-align:left;"> no </td>
   <td style="text-align:left;"> bilat </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 12163 </td>
   <td style="text-align:left;"> F </td>
   <td style="text-align:right;"> 11 </td>
   <td style="text-align:left;"> * </td>
   <td style="text-align:left;"> * </td>
   <td style="text-align:left;"> no </td>
   <td style="text-align:left;"> bilat </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 17076 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:left;"> * </td>
   <td style="text-align:left;"> 1 </td>
   <td style="text-align:left;"> no </td>
   <td style="text-align:left;"> bilat </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 18025 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 60 </td>
   <td style="text-align:left;"> 1 </td>
   <td style="text-align:left;"> * </td>
   <td style="text-align:left;"> affected sib. </td>
   <td style="text-align:left;"> bilat </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 6 </td>
   <td style="text-align:right;"> 18237 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 22 </td>
   <td style="text-align:left;"> * </td>
   <td style="text-align:left;"> * </td>
   <td style="text-align:left;"> no </td>
   <td style="text-align:left;"> bilat </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 7 </td>
   <td style="text-align:right;"> 20291 </td>
   <td style="text-align:left;"> F </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:left;"> 3 </td>
   <td style="text-align:left;"> * </td>
   <td style="text-align:left;"> no </td>
   <td style="text-align:left;"> bilat </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 8 </td>
   <td style="text-align:right;"> 22699 </td>
   <td style="text-align:left;"> F </td>
   <td style="text-align:right;"> 18 </td>
   <td style="text-align:left;"> 2 </td>
   <td style="text-align:left;"> * </td>
   <td style="text-align:left;"> no </td>
   <td style="text-align:left;"> bilat </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:right;"> 24729 </td>
   <td style="text-align:left;"> F </td>
   <td style="text-align:right;"> 30 </td>
   <td style="text-align:left;"> * </td>
   <td style="text-align:left;"> * </td>
   <td style="text-align:left;"> no </td>
   <td style="text-align:left;"> bilat </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 30464 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:left;"> 2 </td>
   <td style="text-align:left;"> * </td>
   <td style="text-align:left;"> no </td>
   <td style="text-align:left;"> bilat </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 11 </td>
   <td style="text-align:right;"> 30470 </td>
   <td style="text-align:left;"> F </td>
   <td style="text-align:right;"> 6 </td>
   <td style="text-align:left;"> * </td>
   <td style="text-align:left;"> 1 </td>
   <td style="text-align:left;"> no </td>
   <td style="text-align:left;"> bilat </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 12 </td>
   <td style="text-align:right;"> 37837 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 7 </td>
   <td style="text-align:left;"> * </td>
   <td style="text-align:left;"> 2 </td>
   <td style="text-align:left;"> affected sib. </td>
   <td style="text-align:left;"> bilat </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:right;"> 41134 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:left;"> 3 </td>
   <td style="text-align:left;"> * </td>
   <td style="text-align:left;"> no </td>
   <td style="text-align:left;"> bilat </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 14 </td>
   <td style="text-align:right;"> 43391 </td>
   <td style="text-align:left;"> F </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:left;"> 5 </td>
   <td style="text-align:left;"> * </td>
   <td style="text-align:left;"> no </td>
   <td style="text-align:left;"> bilat </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 15 </td>
   <td style="text-align:right;"> 44176 </td>
   <td style="text-align:left;"> F </td>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:left;"> * </td>
   <td style="text-align:left;"> * </td>
   <td style="text-align:left;"> no </td>
   <td style="text-align:left;"> bilat </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 16 </td>
   <td style="text-align:right;"> 44649 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 18 </td>
   <td style="text-align:left;"> * </td>
   <td style="text-align:left;"> 4 </td>
   <td style="text-align:left;"> no </td>
   <td style="text-align:left;"> bilat </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 17 </td>
   <td style="text-align:right;"> 46860 </td>
   <td style="text-align:left;"> F </td>
   <td style="text-align:right;"> 24 </td>
   <td style="text-align:left;"> * </td>
   <td style="text-align:left;"> * </td>
   <td style="text-align:left;"> no </td>
   <td style="text-align:left;"> bilat </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 18 </td>
   <td style="text-align:right;"> 59704 </td>
   <td style="text-align:left;"> F </td>
   <td style="text-align:right;"> 44 </td>
   <td style="text-align:left;"> 1 </td>
   <td style="text-align:left;"> * </td>
   <td style="text-align:left;"> no </td>
   <td style="text-align:left;"> bilat </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 19 </td>
   <td style="text-align:right;"> 67024 </td>
   <td style="text-align:left;"> F </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:left;"> * </td>
   <td style="text-align:left;"> * </td>
   <td style="text-align:left;"> no </td>
   <td style="text-align:left;"> bilat </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 20 </td>
   <td style="text-align:right;"> 68422 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 12 </td>
   <td style="text-align:left;"> * </td>
   <td style="text-align:left;"> 1 </td>
   <td style="text-align:left;"> no </td>
   <td style="text-align:left;"> bilat </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 21 </td>
   <td style="text-align:right;"> 69224 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:left;"> * </td>
   <td style="text-align:left;"> * </td>
   <td style="text-align:left;"> no </td>
   <td style="text-align:left;"> bilat </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 22 </td>
   <td style="text-align:right;"> 72656 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 12 </td>
   <td style="text-align:left;"> * </td>
   <td style="text-align:left;"> 1 </td>
   <td style="text-align:left;"> no </td>
   <td style="text-align:left;"> bilat </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 23 </td>
   <td style="text-align:right;"> 74616 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 15 </td>
   <td style="text-align:left;"> 1 </td>
   <td style="text-align:left;"> * </td>
   <td style="text-align:left;"> father and paternal uncle </td>
   <td style="text-align:left;"> bilat </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 24 </td>
   <td style="text-align:right;"> 198 </td>
   <td style="text-align:left;"> F </td>
   <td style="text-align:right;"> 48 </td>
   <td style="text-align:left;"> * </td>
   <td style="text-align:left;"> * </td>
   <td style="text-align:left;"> no </td>
   <td style="text-align:left;"> unilat </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 25 </td>
   <td style="text-align:right;"> 3705 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 22 </td>
   <td style="text-align:left;"> * </td>
   <td style="text-align:left;"> * </td>
   <td style="text-align:left;"> no </td>
   <td style="text-align:left;"> unilat </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 26 </td>
   <td style="text-align:right;"> 6600 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 33 </td>
   <td style="text-align:left;"> * </td>
   <td style="text-align:left;"> * </td>
   <td style="text-align:left;"> no </td>
   <td style="text-align:left;"> unilat </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 27 </td>
   <td style="text-align:right;"> 8847 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 38 </td>
   <td style="text-align:left;"> * </td>
   <td style="text-align:left;"> * </td>
   <td style="text-align:left;"> no </td>
   <td style="text-align:left;"> unilat </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 28 </td>
   <td style="text-align:right;"> 8997 </td>
   <td style="text-align:left;"> F </td>
   <td style="text-align:right;"> 47 </td>
   <td style="text-align:left;"> * </td>
   <td style="text-align:left;"> * </td>
   <td style="text-align:left;"> no </td>
   <td style="text-align:left;"> unilat </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 29 </td>
   <td style="text-align:right;"> 19118 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 50 </td>
   <td style="text-align:left;"> * </td>
   <td style="text-align:left;"> * </td>
   <td style="text-align:left;"> no </td>
   <td style="text-align:left;"> unilat </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 30 </td>
   <td style="text-align:right;"> 24986 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 32 </td>
   <td style="text-align:left;"> * </td>
   <td style="text-align:left;"> * </td>
   <td style="text-align:left;"> no </td>
   <td style="text-align:left;"> unilat </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 31 </td>
   <td style="text-align:right;"> 26961 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 28 </td>
   <td style="text-align:left;"> * </td>
   <td style="text-align:left;"> * </td>
   <td style="text-align:left;"> no </td>
   <td style="text-align:left;"> unilat </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 32 </td>
   <td style="text-align:right;"> 33131 </td>
   <td style="text-align:left;"> F </td>
   <td style="text-align:right;"> 31 </td>
   <td style="text-align:left;"> * </td>
   <td style="text-align:left;"> * </td>
   <td style="text-align:left;"> no </td>
   <td style="text-align:left;"> unilat </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 33 </td>
   <td style="text-align:right;"> 36322 </td>
   <td style="text-align:left;"> F </td>
   <td style="text-align:right;"> 29 </td>
   <td style="text-align:left;"> * </td>
   <td style="text-align:left;"> * </td>
   <td style="text-align:left;"> no </td>
   <td style="text-align:left;"> unilat </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 34 </td>
   <td style="text-align:right;"> 40061 </td>
   <td style="text-align:left;"> F </td>
   <td style="text-align:right;"> 21 </td>
   <td style="text-align:left;"> * </td>
   <td style="text-align:left;"> * </td>
   <td style="text-align:left;"> no </td>
   <td style="text-align:left;"> unilat </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 35 </td>
   <td style="text-align:right;"> 40306 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 46 </td>
   <td style="text-align:left;"> * </td>
   <td style="text-align:left;"> * </td>
   <td style="text-align:left;"> no </td>
   <td style="text-align:left;"> unilat </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 36 </td>
   <td style="text-align:right;"> 41628 </td>
   <td style="text-align:left;"> F </td>
   <td style="text-align:right;"> 36 </td>
   <td style="text-align:left;"> * </td>
   <td style="text-align:left;"> * </td>
   <td style="text-align:left;"> no </td>
   <td style="text-align:left;"> unilat </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 37 </td>
   <td style="text-align:right;"> 45583 </td>
   <td style="text-align:left;"> F </td>
   <td style="text-align:right;"> 73 </td>
   <td style="text-align:left;"> * </td>
   <td style="text-align:left;"> * </td>
   <td style="text-align:left;"> no </td>
   <td style="text-align:left;"> unilat </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 38 </td>
   <td style="text-align:right;"> 46371 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 29 </td>
   <td style="text-align:left;"> * </td>
   <td style="text-align:left;"> * </td>
   <td style="text-align:left;"> no </td>
   <td style="text-align:left;"> unilat </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 39 </td>
   <td style="text-align:right;"> 47070 </td>
   <td style="text-align:left;"> F </td>
   <td style="text-align:right;"> 15 </td>
   <td style="text-align:left;"> * </td>
   <td style="text-align:left;"> * </td>
   <td style="text-align:left;"> no </td>
   <td style="text-align:left;"> unilat </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 40 </td>
   <td style="text-align:right;"> 52892 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 52 </td>
   <td style="text-align:left;"> * </td>
   <td style="text-align:left;"> * </td>
   <td style="text-align:left;"> no </td>
   <td style="text-align:left;"> unilat </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 41 </td>
   <td style="text-align:right;"> 54321 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 24 </td>
   <td style="text-align:left;"> * </td>
   <td style="text-align:left;"> * </td>
   <td style="text-align:left;"> no </td>
   <td style="text-align:left;"> unilat </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 42 </td>
   <td style="text-align:right;"> 61533 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 8 </td>
   <td style="text-align:left;"> * </td>
   <td style="text-align:left;"> * </td>
   <td style="text-align:left;"> no </td>
   <td style="text-align:left;"> unilat </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 43 </td>
   <td style="text-align:right;"> 64465 </td>
   <td style="text-align:left;"> F </td>
   <td style="text-align:right;"> 19 </td>
   <td style="text-align:left;"> * </td>
   <td style="text-align:left;"> * </td>
   <td style="text-align:left;"> no </td>
   <td style="text-align:left;"> unilat </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 44 </td>
   <td style="text-align:right;"> 64622 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 36 </td>
   <td style="text-align:left;"> * </td>
   <td style="text-align:left;"> * </td>
   <td style="text-align:left;"> no </td>
   <td style="text-align:left;"> unilat </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 45 </td>
   <td style="text-align:right;"> 68543 </td>
   <td style="text-align:left;"> F </td>
   <td style="text-align:right;"> 34 </td>
   <td style="text-align:left;"> * </td>
   <td style="text-align:left;"> * </td>
   <td style="text-align:left;"> no </td>
   <td style="text-align:left;"> unilat </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 46 </td>
   <td style="text-align:right;"> 69502 </td>
   <td style="text-align:left;"> F </td>
   <td style="text-align:right;"> 27 </td>
   <td style="text-align:left;"> * </td>
   <td style="text-align:left;"> * </td>
   <td style="text-align:left;"> no </td>
   <td style="text-align:left;"> unilat </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 47 </td>
   <td style="text-align:right;"> 74498 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:left;"> * </td>
   <td style="text-align:left;"> * </td>
   <td style="text-align:left;"> no </td>
   <td style="text-align:left;"> unilat </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 48 </td>
   <td style="text-align:right;"> 76092 </td>
   <td style="text-align:left;"> F </td>
   <td style="text-align:right;"> 8 </td>
   <td style="text-align:left;"> * </td>
   <td style="text-align:left;"> * </td>
   <td style="text-align:left;"> no </td>
   <td style="text-align:left;"> unilat </td>
  </tr>
</tbody>
</table></div>



## Comparing Figure 1 with Table 1

Let's load a screenshot of Figure 1 taken from Knudson, 1971. I've added the word "Original" to the top of the figure so that we know it's the figure directly from the manuscript.
![plot of chunk unnamed-chunk-4](figure/unnamed-chunk-4-1.png)

Of note, the x-axis in Figure 1 ends at 60 months. However, there are 2 patients in Table 1 who are diagnosed at or after age 60 months. 

To make sure we have the right number of data points, let's count the number of data points represented in this figure and compare this with the expected number of data points. 

I've annotated a screenshot of Figure 1 to count the number of points: 

![plot of chunk unnamed-chunk-5](figure/unnamed-chunk-5-1.png)

There are fewer data points than expected based on the legend (20 unilateral and 17 bilateral cases compared with 25 and 23 expected, respectively).

Why are there fewer points in the plot than expected? The y-axis describes the "fraction of cases not yet diagnosed **at the age indicated**." Therefore, it is possible that the author collapsed cases occuring in the same month into a single data point. 

Let's recreate figure 1 with this approach in mind.


## Recreating Figure 1

Let's put the reproduced (labeled as "Reproduced in R") and original figures side by side so that we can compare the two.


``` r

k2 <- k %>%
  arrange(side, -age_at_dx) %>%
  group_by(side) %>%
  mutate(idx_sum = 1 / length(side), 
         prop_of_tot = cumsum(idx_sum) - idx_sum) %>%
  ungroup %>%
  mutate(side=factor(side, levels=c("unilat", "bilat"))) %>%
  group_by(side, age_at_dx) %>%
  ## collapse cases occuring in the same month into a single data point
  filter(prop_of_tot == min(prop_of_tot)) 

y_axis_seq <- c(0.01, seq(0.02, 0.1, 0.02), seq(0.2, 1, 0.2))
y_axis_seq_char = format(y_axis_seq, digits=2)

fig1 <- ggplot(k2, aes(x=age_at_dx, y = prop_of_tot, group=side, shape=side)) +
  scale_shape_manual(values=c(21,22), 
                     labels=c("Unilateral Cases (25)","Bilateral Cases (23)")) + 
  scale_y_log10(breaks = y_axis_seq, 
                limits = c(0.01, 1.04), 
                labels = as.character(gsub("^0\\.", "\\.", y_axis_seq_char)),
                expand = c(0, 0)) +
  scale_x_continuous(breaks = seq(0,60, 10), 
                     limits = c(-0.1, 75), 
                     expand = c(0, 0)) +
  xlab("AGE IN MONTHS") + 
  ylab("FRACTION OF CASES NOT YET DIAGNOSED AT AGE INDICATED") +
  theme_classic() + 
  theme(aspect.ratio = 1.5, 
        legend.position = c(0.3,0.16), 
        legend.title=element_blank(),
        axis.title = element_text(size=8)) +
  ggtitle("Reproduced in R") + 
  geom_abline(intercept = 0, slope = -1/30, color="black", 
                 linetype="solid", size=0.5) + 
  stat_function(fun = function(t) -4 * 10^{-4} *t^2, linetype="longdash", color="black") +
  geom_point(fill="white")

k2_missing <- k2 %>% filter(age_at_dx >= 60 | {age_at_dx == 47 & side == "unilat"})

fig1_w_missing <- fig1 + 
  geom_point(data = k2_missing, aes(x=age_at_dx, y = prop_of_tot), pch=21, fill=NA, color = "red", size=4.5)


fig1_w_missing + fig1_orig + plot_layout(widths = c(1, 1.45))
```

![plot of chunk unnamed-chunk-6](figure/unnamed-chunk-6-1.png)

Not bad. We can see that the figure is largely reproducible. However, there are a few differences that we'll highlight in the following sections.

## Missing data

If we look closely, there are 3 data points (highlighted with red circles) in the reproduced figure that differ from the original Figure 1. 

  - First, look along the base of the x-axis in the reproduced figure above. Two data points from patients age >=60 -- the oldest patients to have been diagnosed from each group -- are missing. 
  
  - Note that in the original figure, the x-axis stops at 60, which can explain for the first two missing data points. Perhaps Knudson intentionally excluded these two data points whose y value equals 0, as plotting a 0 value on a log scale is difficult (because log(0) is undefined). 
  
  - For some reason, in the reproduced plot, R plots the top half of these two data points, when really they should not be showing up here at 0.01 (their y value equals 0 on a linear scale, and is undefined on a log scale).

  - A third data point for unilateral retinoblastoma patient 08997, age 47 months, is also missing. However, the reason why this data point is missing is unclear.


## Fitted curves

The black solid line in the reproduced plot above describes data from bilateral cases and could be reproduced exactly. This equation corresponds to the line $logS = -t/30$, as described in the figure 1 legend:


![plot of chunk unnamed-chunk-7](figure/unnamed-chunk-7-1.png)

However, the mathematical equation describing the unilateral cases did not match the curve. The curve $logS = -4 \times 10^{-4}t^2$ (black dotted line) is a much better fit than the curve $logS = -4 \times 10^{-5}t^2$ (red dotted line) described in figure 1 legend.


``` r
fig1 + 
  stat_function(fun = function(t) -4 * 10^{-5} *t^2, linetype="longdash", color="red") +
  annotate("text", x = 60, y = 0.9, label = "-4~x~10^{-5}~t^{2}", parse = TRUE, color="red") + 
  annotate('text', x = 45, y = 0.4, label = "-4~x~10^{-4}~t^{2}", parse = TRUE)
```

![plot of chunk unnamed-chunk-8](figure/unnamed-chunk-8-1.png)

This suggests that the exponent from the equation reported in the figure 1 legend is off by a digit (should be $10^{-4}$ instead of $10^{-5}$).

## Conclusions

- Overall, it was satisfying to see that Figure 1 from Knusdon 1971 is highly reproducible from the data in Table 1.

- However, there were a few key exceptions:  

    - There was 1 missing and unexplained data point in Figure 1.

    - The fitted curve for the unilateral cases has an error in the exponent.

- These inconsistencies do not change the conclusions of this paper.

- Also, notice that this paper was ['communicated'](https://www.pnas.org/content/106/37/15518) to PNAS and did not undergo standard peer review. This could have opened the door for the minor unchecked mistakes we observed.

    - Fortunately, PNAS [discontinued communicated manuscripts](https://www.pnas.org/content/106/37/15518) in 2010

Please feel free to contact me at dpique1@gmail.com with any questions or comments! 

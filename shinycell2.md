# ShinyCell2

Interactive web applications for single-cell data. (LIS fork)

### Links

[GitHub repository](https://github.com/legumeinfo/ShinyCell2) -
See the `README` for discussion of our LIS changes.

#### Branches

main<br>
Fixes a 'deprecated argument' warning [(pull request)](https://github.com/the-ouyang-lab/ShinyCell2/pull/15).

navbar-fix<br>
Cleans up some problems with the application layout [(pull request)](https://github.com/the-ouyang-lab/ShinyCell2/pull/17).

lis<br>
Our LIS changes.

#### Running instances

_Medicago truncatula_ - _Meliloti_ vs. Mock Inoculated Root
<br/>https://shinycell.legumeinfo.org/medtr.A17.gnm5.ann1_6.expr.Cervantes-Perez_Thibivilliers_2022-shinycell2/
<!-- <br/>(dev) http://dev.lis.ncgr.org:50088 -->

_Glycine max_ - Nodules vs. Root Seedlings
<br/>https://shinycell.legumeinfo.org/glyma.Wm82.gnm2.ann1.expr.Cervantes-Perez_Zogli_2024-shinycell2/
<!-- <br/>(dev) http://dev.lis.ncgr.org:50089 -->

### Notes

R scripts for building our specific applications live under
```
/falafel/svengato/shinycell/<your-ShinyCell2-application>/build/
```
while their Docker files live under
```
/falafel/svengato/shinycell/<your-ShinyCell2-application>/docker/
```

### To do

Add **more** datasets for **more** species.

Ability to POST a longer list of genes as default choices for the Gene Name dropdown menus, and use genes[1] and genes[2] in place of gene1 &amp; gene2.

Fix whatever is causing the warning messages (visible in the R console when running locally, and probably in the shiny-server logs).
**Update:** The pull requests mentioned above would fix some of these.

Get it to handle ATAC-seq data and generate the Track Plot panel.

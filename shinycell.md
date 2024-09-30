# ShinyCell

Interactive web applications for single-cell data. (LIS fork)

### Links

[GitHub repository](https://github.com/legumeinfo/shinycell) -
See the `README` for discussion of our LIS changes.

Running instances:

_Medicago truncatula_ - _Meliloti_ vs. Mock Inoculated Root
<br/>https://shinycell.legumeinfo.org/medtr.A17.gnm5.ann1_6.expr.Cervantes-Perez_Thibivilliers_2022/
<br/>(dev) http://dev.lis.ncgr.org:50080/shiny/medtr.A17.gnm5.ann1_6.expr.Cervantes-Perez_Thibivilliers_2022/

_Glycine max_ - Nodules vs. Root Seedlings
<br/>https://shinycell.legumeinfo.org/glyma.expr.Cervantes-Perez_Zogli_2024/
<br/>(dev) http://dev.lis.ncgr.org:50080/shiny/glyma.expr.Cervantes-Perez_Zogli_2024/

### Notes

R scripts for building our specific applications now live in `LIS-apps/`.

### To do

Add **more** datasets for **more** species.

Update the URL on the fly after a user selection, to enable copying the application state to send to colleagues.

Ability to POST a longer list of genes as default choices for the Gene Name dropdown menus, and use genes[1] and genes[2] in place of gene1 &amp; gene2.

Fix whatever is causing the warning messages (visible in the R console when running locally, and probably in the shiny-server logs).

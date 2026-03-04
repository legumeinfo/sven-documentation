# ShinyCell2

Interactive web applications for single-cell data. (LIS fork)

### Links

[GitHub repository](https://github.com/legumeinfo/ShinyCell2) -
See the `README` for discussion of our LIS changes.

Running instances:

_Medicago truncatula_ - _Meliloti_ vs. Mock Inoculated Root
<br/>(dev) http://dev.lis.ncgr.org:50083

_Glycine max_ - Nodules vs. Root Seedlings
<br/>(dev) http://dev.lis.ncgr.org:50084

### Notes

R scripts for building our specific applications live under
```
/falafel/svengato/<your-ShinyCell2-application>/build/
```
while their Docker files live under
```
/falafel/svengato/<your-ShinyCell2-application>/docker/
```

### To do

Add **more** datasets for **more** species.

Ability to POST a longer list of genes as default choices for the Gene Name dropdown menus, and use genes[1] and genes[2] in place of gene1 &amp; gene2.

Fix whatever is causing the warning messages (visible in the R console when running locally, and probably in the shiny-server logs).
**Update:** [These](https://github.com/the-ouyang-lab/ShinyCell2/pull/15) [two](https://github.com/the-ouyang-lab/ShinyCell2/pull/17) pull requests would fix some of these.

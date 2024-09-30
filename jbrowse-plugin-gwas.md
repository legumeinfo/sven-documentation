# JBrowse GWAS plugin

Plugin for displaying GWAS results such as Manhattan plot renderings.

However, it only detects the x coordinate (position along the chromosome) on a mouseover.
There may be multiple SNPs at that position, so we would like it to detect the y coordinate as well, as in ZZBrowse.

### Links

[Original GitHub repository](https://github.com/cmdcolin/jbrowse-plugin-gwas)

[Issue](https://github.com/cmdcolin/jbrowse-plugin-gwas/issues/11)

[My fork](https://github.com/legumeinfo/jbrowse-plugin-gwas/tree/detect-y-mouseover) (detect-y-mouseover branch)

Also requires minor changes to [jbrowse-components](https://github.com/GMOD/jbrowse-components) (JBrowse 2), so I made a
[fork](https://github.com/legumeinfo/jbrowse-components/tree/gwas-plugin-tweaks) of that (gwas-plugin-tweaks branch).

### Notes

`config-lis.json` is set up to use LIS data like those in ZZBrowse (genomes, annotations, GWAS, and QTL).
A copy of the `legume-data/` directory is at `/falafel/svengato/legume-data/`.

`traitcolorplugin.js`: plugin for generating a color from the trait name, for GWAS or QTL data.

Things you can format through [jexl](https://www.jbrowse.org/jb2/docs/config_guides/jexl/) in `config.json`:
number of digits in p-value; tooltip contents (on mouseover).

### Build and run the plugin

First build (once, unless you change it) and run jbrowse-components,
```
$ cd [...]/jbrowse-components
$ yarn

$ cd [...]/jbrowse-components/products/jbrowse-web
$ yarn start
```
To run this version of JBrowse by itself, go to http://localhost:3000

To build and run the GWAS plugin,
```
$ cd [...]/jbrowse-plugin-gwas
$ yarn

$ yarn start
```
Then go to http://localhost:3000/?config=http://localhost:9000/config-lis.json


### To do

Get it to detect the y coordinate on mouseover, if possible.

`traitcolorplugin.js`: Decide on a preferred hashing method and remove the others.
`crypto.subtle.digest()` requires HTTPS?

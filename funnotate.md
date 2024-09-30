# Funnotate

Functional annotation of FASTA sequences. Can also search for gene families using functional keywords.

### GitHub branches

[main](https://github.com/legumeinfo/Funnotate) -
Original uncontainerized version, running under shiny-server. Uses the legfed_v1_0.L_xxxxxx gene families.

[docker-sgr](https://github.com/legumeinfo/Funnotate/tree/docker-sgr) -
First cut at a containerized version. Uses the legfed_v1_0.L_xxxxxx gene families.

[new-gene-families](https://github.com/legumeinfo/Funnotate/tree/new-gene-families) -
Containerized version that uses the Legume_fam3_nnnnn gene families, with species color coded by taxon-symbology 1.1.0.

I think the idea is not to eventually merge the branches, but to choose one in `docker-compose.yml`.

### Running instances

http://dev.lis.ncgr.org:50080/shiny/Funnotate/ -
Development version (main) on sgr-shiny

https://funnotate.legumeinfo.org -
Production version (main)

http://dev.lis.ncgr.org:50082 -
Development version (new-gene-families, but you could also switch back to docker-sgr) on sgr-funnotate

### Notes

See the [README](https://github.com/legumeinfo/Funnotate) file for a brief description (and flowchart), plus explanations of how to set URL fields and examples.

#### Code notes

`backend.R`: We use the R `InterMineR` package to access LegumeMine (for gene family keyword search, and looking up proteins and gene families associated with genes). However, `InterMineR` is deprecated starting in Bioconductor 3.20. Workaround: stick to Bioconductor 3.19.

`ui.R`: Uses a nonstandard syntax that supports both GET and POST requests. This is so other applications, like mines, can POST sequences to Funnotate.

#### Data notes

`static/upload/` - Data for uploaded FASTA files go here. `input_n` are the FASTA sequences, `upload_n` are their corresponding metadata.

`static/job/` - Data for jobs go here. `job_xxxxx` are the job metadata, the others are output files.

`static/examples/` - Any FASTA files you store here will appear as examples on the Home page.
This was Sudhansu's idea, he did not want to have to hunt for his favorites, and make several clicks to load them, while giving a conference presentation.
However, the example FASTA files on the running instances above are not Sudhansu's, they are just random sequences I got from LegumeMine, so you are welcome to substitute more meaningful ones.

`static/error.log` - Check here as well as the `/var/log/shiny-server/Funnotate*` log files to diagnose a problem.

#### Application notes

InterProScan: The containerized versions use the InterProScan service at `/falafel/adf/sw/interproscan-5.60-92.0`, which is faster than the old one used by the uncontainerized version.
To change this, edit `settings.yml`.

Rename data directories on falafel to `data_<gene_families>_<hostname>`, then specify which gene families to use in `docker-compose.yml` (this affects the Lorax data installer script)

### To do

See also [Issues](https://github.com/legumeinfo/Funnotate/issues) in the GitHub repository.

On a job page, display `blast_`, `.tbl`, `.trans` as text files instead of asking the user to download them.
<br>&rarr; Could configure in `/srv/shiny-server/Funnotate/static/job/.htaccess`? Requires Shiny Server Pro (or its Posit equivalent)?

Improve error checking for input sequences.

More linkouts to internal nodes.

Update window title on the fly?
<br/>`runjs(sprintf("document.title = 'Funnotate keyword search: %s';", input$familyKeywords))`

**Tour**
<br/>Dialog sometimes does not go away at the end
<br/>Difficulty clicking Next/Previous/Close buttons (due to `driver.js` issues on Linux?)
<br/>Reset Taxa, MSA previous state after closing Tour

Make phylogram panels movable (maybe with [GoldenLayout](https://golden-layout.com)?)

Decorate tree nodes as in phylotree.org (or add to linkouts?) &rarr; Warning: disagreement between GO terms?

Include phylotree web component (if one can add them to Shiny applications) (and if it replicates the behavior of `static/js/phylogram.js`).

Replace PANTHER service for InterPro? This would go in the 'interpro' section of `settings.yml`.

We ignore the `isPrimary` attribute for proteins, CDs, mRNA coming from mines. Should we?

Check accuracy/consistency of MSA when user sequences exist. Trimmed v. untrimmed sequences? Andrew already updated this on funnotate-vm, should eventually update on sgr-shiny and sgr-funnotate if it has not been done already.

Decorate tree nodes by function?

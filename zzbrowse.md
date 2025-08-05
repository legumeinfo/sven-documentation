# ZZBrowse

ZZBrowse is our comparative genomics extension of ZBrowse.

### Links

[GitHub repository](https://github.com/legumeinfo/ZZBrowse/tree/dev-legfedorg) (dev-legfedorg branch)

[Wiki](https://github.com/legumeinfo/ZZBrowse/wiki) - Mostly user, some developer and data manager documentation. Look here first.

[LIS weblog](https://www.legumeinfo.org/blog/2022/02/17/zzbrowse.html)

Articles
&bull; [Berendzen _et al._](https://onlinelibrary.wiley.com/doi/10.1002/leg3.74)
&bull; [Redsun _et al._](https://link.springer.com/protocol/10.1007/978-1-0716-2067-0_4)

### Running instances

[Development](http://dev.lis.ncgr.org:50071/shiny/ZZBrowse/) (on dev-zzbrowse, 50071)

[Production](https://zzbrowse.legumeinfo.org)

### To do

[Issues](https://github.com/legumeinfo/ZZBrowse/issues) in the GitHub repository

**Deprecate in favor of JBrowse 2.** This requires improvements to the [JBrowse GWAS plugin](jbrowse-plugin-gwas.md), or writing our own similar plugin.

Switch from local (as on laptops) to remote (as on dev/prod VMs) Highcharts script.

Consistent (as opposed to contrasting) trait colors: implemented locally by hashing trait name.

New species: lentil, lima bean, etc.

Display trait ontology hierarchy somehow.

Handle multiple genomes (and annotations?) - allow user to select a genome?

How to scp to zzbrowse-vm? (neither svengato nor legumista have a home directory)

Other performance issues

#### To do: Data process

Is the Chiteri mung bean dataset (in data store) up to date?

Update genome annotations for all species, see whether this changes the data -
<br/>missing linkouts, missing macrosynteny data, different chromosome names:
```
A. hypogaea    gnm1.ann1.CCJH (Arahy.01) -> gnm2.ann2.PVFB (chr01)
C. cajan       gnm1.ann1.Y27M (Cc01)     -> gnm2.ann1.L3ZH (chr01)
G. max         gnm2.ann1.RVB6 (Gm01)     -> gnm4.ann1.T8TQ (Gm01)
M. truncatula  gnm4.ann2.G3ZY (chr1)     -> gnm5.ann1_6.L2RX (Chr1, no leading .)
P. vulgaris    gnm1.ann1.pScz (Chr01)    -> gnm2.ann1.PB8d (Chr01)
V. radiata     gnm7.ann1.RWBG (chr1)
V. unguiculata gnm1.ann1.zb5D (Vu01)     -> gnm1.ann2.FD7K (Vu01)
```

Add more reporting to data process (automated data processing + error tracking)
```
Detect missing and duplicate markers
  For each GWAS file {
    For each GWAS row {
      Find markers (or note they are missing or duplicate)
      GWAS data, GWAS file, marker(s), marker file(s)
    }
  }
  Detect which marker files are not used, and comment them out
    setdiff(unique(all marker files), unique(used marker files))
  Questions
    Can markers have multiple positions (e.g. across chromosomes)?
    Are marker files associated with a single genome?
```

Separate the data process from ZZBrowse?

Review and add data process/update instructions to [datastore Protocols document](https://github.com/legumeinfo/datastore-specifications/blob/main/PROTOCOLS/README.md)

#### To do: Macrosynteny issues

Profile macrosynteny blocks computation to try to speed it up

Compute only once per pair of datasets

Put macrosynteny block selections in URL? (probable conflicts)

Separate species 1 blocks more (vertically)

Use `httr` instead of `RCurl`? (~5% speedup)

On macrosynteny block selection, do not recenter microsynteny region if it exists (or vice versa?)

"Macro-synteny" v. "Macrosynteny"

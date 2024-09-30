# Lorax

Web server that computes phylogenetic trees for gene families. Used by [Funnotate](funnotate.md).

### GitHub branches

[main](https://github.com/legumefederation/lorax) -
Original containerized version. Uses the `legfed_v1_0.L_xxxxxx` gene families.

[separate-gene-family-data](https://github.com/LegumeFederation/lorax/tree/separate-gene-family-data) -
Gene family data now stored in a bind-mounted subdirectory on `falafel`.
Should have the form `/falafel/lorax/data_<gene_family_subdirectory>_<hostname>`,
for example `data_legume.fam1.M65K_sgr-funnotate` which is where the `legfed_v1_0.L_xxxxxx` gene family data live.

[new-gene-families](https://github.com/legumefederation/lorax/tree/new-gene-families) -
Uses the `Legume_fam3_nnnnn` gene families, in `data_legume.fam3.VLMQ_sgr-funnotate`.

I think the idea is not to eventually merge the branches, but to choose one in `docker-compose.yml`.

### Other Links

[Lorax URLs](https://github.com/LegumeFederation/lorax/blob/master/docs/urls.rst)


### Running instances

Check out the appropriate branch and run `sudo docker compose up -d [--build] [--force-recreate]`.

Currently running on sgr-shiny, funnotate-vm, and sgr-funnotate.

### Installing pristine data

For the newer versions that store data on `/falafel`,
```
$ cd /falafel/lorax/data_<gene_family_subdirectory>_<hostname>
$ sudo ../install_families.sh
```

For the old version,
```
Enter the Lorax container
$ sudo docker exec -ti lorax_flask_1 bash

Inside the container,
$$ export LORAX_CURL_URL=flask:8000
$$ cd /usr/local/var/data

Turn off phytozome families, if necessary
$$ touch phytozome_10_2.done

Install
$$ /usr/src/app/lorax/test/install_families.sh
```

Each gene family subdirectory should initially contain these files:
* `alignment.faa` (multiple sequence alignment, in FASTA format)
* `family.hmm` (HMM data)
* `hmmstats.json` (HMM metadata)
* `sequence_data.json` (sequence metadata)
* `sequences.faa` (FASTA sequences)
* `FastTree/tree.nwk` (Newick tree)

# ArachisPheno

ArachisPheno is our fork of [AraPheno](https://arapheno.1001genomes.org) for _Arachis_ phenotype data.
I started to generalize it to other species, like MedicPheno for alfalfa.

### GitHub branches

[master](https://github.com/legumeinfo/ArachisPheno)
This branch contains common code, with the original "arapheno" changed to "xxxpheno".

[arachispheno](https://github.com/legumeinfo/ArachisPheno/tree/arachispheno)
Customized for _Arachis_, for example it would use "arachispheno" instead of "xxxpheno".

[medicpheno](https://github.com/legumeinfo/ArachisPheno/tree/medicpheno)
Started customization for alfalfa, but it does not work?

### Running instances

[Development](http://dev.lis.ncgr.org:50007) (dev-arachispheno, port 50007) -
Contains the peanut minicore data. Not currently running, the VM is up but the reverse proxy was missing. I created one, but it needs work.

[Production](https://arachispheno.peanutbase.org) (arachispheno-vm) - Also contains the peanut minicore data.

### Notes

[Wiki](https://github.com/legumeinfo/ArachisPheno/wiki) with developer notes!

Both VMs have an `ArachisPheno-core/` and an `ArachisPheno-minicore/` directory, each with their own data and Docker settings.
You have to choose one to build at a given time. Currently both are using the minicore data.

**minicore** are the public data, **core** were (still are?) unpublished data for which we required user authentication.
Where to specify whether authentication is required: `xxxpheno/xxxpheno/settings/private_settings.py`

In `ArachisPheno-minicore/`, the `customize.sh` script lets you change file names for your project.

### To do

Finish genus customization, get MedicPheno working.

Remove some commented-out code describing AraPheno project sponsors.

Two sections from the original AraPheno are commented out: **Download Database** and **Take a Tour**. Could add them back in.

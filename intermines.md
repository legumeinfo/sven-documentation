# InterMines

LIS InterMines arcana. I mainly worked on trying to update 5.1.0.3 development mines to 5.1.0.4.

### Links

[Mine-related issues](https://github.com/legumeinfo/mine-issues/issues)

Submodules:

[dbmodel/resources](https://github.com/legumeinfo/mine-dbmodel-resources/tree/5.1.0.4)

[libs](https://github.com/legumeinfo/mine-libs/tree/5.1.0.4)

[webapp/src/main/java](https://github.com/legumeinfo/mine-java/tree/5.1.0.4)

[webapp/src/main/resources](https://github.com/legumeinfo/mine-webapp-resources/tree/5.1.0.4)

[webapp/src/main/webapp](https://github.com/legumeinfo/mine-webapp/tree/5.1.0.4)

### Updating mines from 5.1.0.3 to 5.1.0.4

On lupini-mines, I have done this for all except arachismine, glycinemine, legumemine, and minimine.

To check which mines are deployed: `ls -l /var/lib/tomcat/webapps/`

Example: LensMine

On lupini-mines, as legumista,
```
$ cd /home/legumista/lensmine
```

To have it prompt for remote actions (instead of using Sam's old credentials),
```
$ git config --local credential.helper ""
```

Edit the overall properties file
<br/>`$ vi ../.intermine/lensmine.properties`
<br/>Change "5.1.0.3" to "5.1.0.4"
<br/>Change "5103" to "5104"
<br/>Change "shokin@..." to "mines@..."
<br/>Change webapp.hostname to "localhost"
<br/>Change mail.host to "lis.ncgr.org"
<br/>Delete "shokin-webapps"
<br/>Change superuser.account to <adf's e-mail>, which also requires adf to set up a user account for each mine.

Create the 5.1.0.4 branch [if none already exists]
```
$ git checkout 5.1.0.3
$ git checkout -b 5.1.0.4
```

Create the 5.1.0.4 database (not those that already exist, like items-lensmine)
```
$ psql -h papadum -l
$ createdb -h papadum -U svengato -O intermine lensmine-5104
```

Set all submodules to 5.1.0.4, and pull latest changes.

Edit `web.properties`:
<br/>`$ vi webapp/src/main/web.properties`
<br/>Change "5.1.0.3" to "5.1.0.4"
<br/>Add taxon ids if necessary, see the [Taxonomy Browser](https://www.ncbi.nlm.nih.gov/Taxonomy/Browser/wwwtax.cgi?mode=Info&id=3864&lvl=3&lin=f&keep=1&srchmode=1&unlock)
<br/>Standardize 'thirdBox' and meta.description text
<br/>Remove 'shokin' references if any.
<br/>Add legendPosition options if missing.

Change "5.1.0.3" to "5.1.0.4" in `gradle.properties` and `.gitmodules`.

Update location of data files (to `falafel`)
```
$ sed -i "s/\/home\/shokin\/data/\/home\/legumista\/data/g" project.xml
$ sed -i "s/\/home\/shokin/\/falafel\/legumeinfo\/rsyncd_datastore/g" project.xml
```

Edit `project.xml`:
<br/>Update gene families from "legume.genefam.fam1.M65K" to "legume.fam1.M65K"
<br/>Comment out trees (for now)
<br/>Check for new datastore files, and add them
<br/>Remove the line with default.intermine.properties.file if it exists (?)

Add missing species to `dbmodel/resources/organism_config.properties`.

[Until it gets committed] Update libs/ncgr-datastore.jar
```
$ cp ~/java/ncgr/datastore/build/libs/ncgr-datastore.jar libs/.
```

### Build the mine
```
$ ./gradlew clean
$ ./gradlew buildDB
$ ./gradlew integrate --stacktrace -DmaxGenesWithoutNotes=-1
$ ./gradlew postprocess --stacktrace
```

### Solr commands (only required once or with major changes?)

Use Solr to initialize search indexes (should not be necessary?)

First set the solr URL (localhost -> solr) in
```
 dbmodel/resources/keyword_search.properties
 dbmodel/resources/objectstoresummary.config.properties
```

Login to jicama, then solr (as legumista).
```
$ sudo su - solr
$ cd /var/solr/data
#$ /opt/solr/bin/solr start (not necessary, it is already started)
$ /opt/solr/bin/solr create -c lensmine-search
$ /opt/solr/bin/solr create -c lensmine-autocomplete
```

Then back on lupini-mines `/home/legumista/lensmine`,
```
$ ./gradlew postprocess -Pprocess=create-search-index
BUILD SUCCESSFUL in 4m 41s
8 actionable tasks: 6 executed, 2 up-to-date
```

May need to restart solr at this point (on solr, `sudo systemctl restart solr`)
```
$ ./gradlew postprocess -Pprocess=create-autocomplete-index
BUILD SUCCESSFUL in 17s
8 actionable tasks: 5 executed, 3 up-to-date
```

### Run the mine

Deploy to Tomcat
```
$ ./gradlew --stacktrace cargoRedeployRemote [or cargoDeployRemote the first time?]
BUILD SUCCESSFUL in 26s
18 actionable tasks: 16 executed, 2 up-to-date
```

It lives at https://mines.dev.lis.ncgr.org/lensmine/begin.do

### Commit to repository when satisfied

Finally, add files to repository
```
$ git add <files from above>
```

Commit like this:
```
$ git -c user.name="<your name>" -c user.email="<your e-mail>" commit -m "<Commit message>"
```

Push to remote
```
$ git push [-u] origin main
```

Update the submodules so `git status` does not display them under "Changes not staged for commit"
```
$ git add <submodule path(s)>
$ git -c user.name="<your name>" -c user.email="<your e-mail>" commit -m "Update submodule(s)"
$ git push
```

### Notes on individual mines

AeschynomeneMine: It works, despite a few messages like
```
WARNING: sun.reflect.Reflection.getCallerClass is not supported. This will impact performance.
```

ArachisMine: adf is working on it.

CajanusMine: It works.

CicerMine:
```
  Failed to create a Non-Pooling DataSource from PostgreSQL JDBC Driver 42.2.2 for intermine at jdbc:postgresql://papadums
  SQLException occurred while connecting to papadum:5432
  Connection error:
  org.postgresql.util.PSQLException: FATAL: remaining connection slots are reserved for non-replication superuser connects
  [...]
  Caused by: java.lang.reflect.InvocationTargetException
        at java.base/jdk.internal.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
        at java.base/jdk.internal.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
        at java.base/jdk.internal.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
        at org.intermine.objectstore.ObjectStoreFactory.getObjectStore(ObjectStoreFactory.java:68)
        at org.intermine.dataloader.ObjectStoreDataLoaderTask.execute(ObjectStoreDataLoaderTask.java:127)
        ... 135 more
  Caused by: java.lang.IllegalArgumentException: ObjectStore 'os.common-translated-std' not found in properties
  BUILD FAILED in 3h 4m 33s
  9 actionable tasks: 7 executed, 2 up-to-date
  -> Really due to excess connections, see https://github.com/intermine/intermine/issues/912
```

GlycineMine: Not updated.

LegumeMine: Not updated.

LensMine: It works.

LupinusMine:
```
  FAILURE: Build failed with an exception.
  * What went wrong:
  Execution failed for task ':dbmodel:integrateMultipleSources'.
  > java.lang.RuntimeException: Exon lupal.Amiga.gnm1.ann1.ncRNA:Lalb_Chr00c01g0403621.ncrna0 parent mRNA lupal.Amiga.gnm1.ann1.ncRNA:Lalb_Chr00c01g0403621 <has not yet been loaded. Is the GFF sorted?
```

MedicagoMine - see mine-issues #169

MiniMine - Sam already updated it to 5.1.0.4

PhaseolusMine:
```
Retrieving G27455.gnm1.ann1.JD7C in a tgt items database
[convertFile] --------------------------------------------------------------------------------
[convertFile] ## Validating phalu collection G27455.gnm1.ann1.JD7C
[convertFile]  - phalu.G27455.gnm1.ann1.JD7C.gene_models_main.gff3.gz
[convertFile]  x phalu.G27455.gnm1.ann1.JD7C.gene_models_main.gff3.gz 20868 gene record Note attributes are missing GO 
[convertFile]  - phalu.G27455.gnm1.ann1.JD7C.protein.faa.gz
[convertFile]  - phalu.G27455.gnm1.ann1.JD7C.protein_primary.faa.gz
[convertFile]  - phalu.G27455.gnm1.ann1.JD7C.cds.fna.gz
[convertFile]  - phalu.G27455.gnm1.ann1.JD7C.cds_primary.fna.gz
[convertFile]  - phalu.G27455.gnm1.ann1.JD7C.mrna.fna.gz
[convertFile]  - phalu.G27455.gnm1.ann1.JD7C.mrna_primary.fna.gz
[convertFile]  - phalu.G27455.gnm1.ann1.JD7C.iprscan.gff3.gz
[convertFile]  - phalu.G27455.gnm1.ann1.JD7C.legfed_v1_0.M65K.gfa.tsv.gz
[convertFile] ## INVALID: GFA file contains gene ID phalu.G27455.gnm1.ann1.Pl10G0000110300 that is not present in the g.
[convertFile] ## INVALID: GFA file contains protein ID phalu.G27455.gnm1.ann1.Pl10G0000110300.1 that is not present in .
[convertFile]  x optional phytozome_10_2.HFNR.gfa.tsv.gz file is not present.

> Task :dbmodel:integrateMultipleSources FAILED

FAILURE: Build failed with an exception.

* What went wrong:
Execution failed for task ':dbmodel:integrateMultipleSources'.
> java.lang.RuntimeException: Collection /falafel/legumeinfo/rsyncd_datastore/v2/Phaseolus/lunatus/annotations/G27455.g.

* Try:
Run with --info or --debug option to get more log output. Run with --scan to get full insights.

* Exception is:
org.gradle.api.tasks.TaskExecutionException: Execution failed for task ':dbmodel:integrateMultipleSources'.
        at org.gradle.api.internal.tasks.execution.ExecuteActionsTaskExecuter.lambda$executeIfValid$3(ExecuteActionsTas)
        at org.gradle.internal.Try$Failure.ifSuccessfulOrElse(Try.java:268)
```

TrifoliumMine: ran out of database connections?

ViciaMine: It works.

VignaMine:
```
[convertFile] ## Processing vigan.Gyeongwon.gnm3.ann1.3Nz5.gene_models_main.gff3.gz

> Task :dbmodel:integrateMultipleSources FAILED

FAILURE: Build failed with an exception.

* What went wrong:
Execution failed for task ':dbmodel:integrateMultipleSources'.
> java.lang.RuntimeException: Exon vigan.Gyeongwon.gnm3.ann1.Vang0010ss00020.1:exon:16254 parent mRNA vigan.Gyeongwon.g?
```

### To do

(O Socrates,) What really are the difference(s) between 5.1.0.3 and 5.1.0.4?
<br/>&rarr; Stick with 5.1.0.3 for now, 5.1.0.4 will eventually use GraphQL which is not yet stable.

Is there a date for 5.1.0.4 (like Aug 2023 for 5.1.0.3)?
<br/>&rarr; adf says no.

Automate creation of `project.xml` from the data store? (low priority/more experience required)

"Welcome Back" box needs more height (say 210 to 280?), can set in mine-webapp `css/begin.css`, line 241 or 248.

Add coexpression data to model, see what phytozome did (create appropriate XML and loaders)? Andrew and Sudhansu could use this.

Option to disregard features in the GFF (by type)? See issue [#169](https://github.com/legumeinfo/mine-issues/issues/169), also [#168](https://github.com/legumeinfo/mine-issues/issues/168).

Increase database connections? Currently disabled most development mines to reduce the need.

Set limit on non-coding genes for each element of `project.xml`, instead of (or in addition to) in the command line?

Restore gene family trees in `project.xml` once they are settled.

5.1.0.4 commits (including submodules) when satisfied.

Issues with "friendly mines" (based on gene families in LIS version, not gene homologues):
standardize; fix issue [#173](https://github.com/legumeinfo/mine-issues/issues/173)

Can one tell from the database which `project.xml` entities have been loaded?

VignaMine: fix issues with expression data (3 Apr 2024); add new genomes to `project.xml`

### Already done

Created development TrifoliumMine and ViciaMine (on lupini-mines).

Display embedded JBrowse 2, with default tracks from linkout.

Allow disabling the check for noncoding genes, like
```
./gradlew integrate --stacktrace -DmaxGenesWithoutNotes=-1
```

Added various missing species to `dbmodel/resources/organism_config.properties`.

Removed import of GWASResult (in mine-java).

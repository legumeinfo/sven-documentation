# lis_gis-brapi

LIS Germplasm Map with accession data from a BrAPI endpoint.

[GitHub repository](https://github.com/legumeinfo/lis_gis/commits/feature/brapi_accessionDetail-sgr)
(feature/brapi_accessionDetail-sgr branch)

### Running instance

http://sgr-lisgis.lis.ncgr.org:8000

### Notes

Files in `/falafel/svengato/brapi-to-postgresql/`
* `accessions_only_new.csv` - Table of accessions that appear only in the new data (from BrAPI).
* `accessions_only_old.csv` - Table of accessions that appear only in the old data.
* `brapi-to-postgresql.R` - R script for acquiring data from the BrAPI endpoint.
<br/>Note that the `brapi-to-postgresql.R` in the repository is newer and thus implicitly more authoritative, though they do pretty much the same thing.
* `lis_germplasm.sql.gz` - Database dump with the new data.
* `notes.txt` - Notes on the data process.

### Populating the database

(from `notes.txt`)

Get a compressed dump of the existing/default database
```
$ wget -O lis_germplasm.sql.xz https://ars-usda.box.com/shared/static/ambc9t4xorieg8qd8e065x9r6dqtua20.xz
```
Then expand the compressed dump (however you like) to `lis_germplasm.sql`

Create lis_germplasm database
```
$ sudo su - postgres
# psql
## CREATE DATABASE lis_germplasm;
## \c lis_germplasm
## CREATE EXTENSION postgis;
## ALTER DATABASE lis_germplasm OWNER TO [your_username];
## \q
# exit
```

Restore from the default database dump
```
$ psql -U [your_username] lis_germplasm < lis_germplasm.sql
```

Change permissions
```
$ sudo su - postgres
# psql
## \c lis_germplasm
## ALTER TABLE spatial_ref_sys OWNER TO [your_username];
```

Check number of rows in the original accession table (from the default database)
```
## SELECT COUNT(*) FROM lis_germplasm.grin_accession;
139477
```

Delete all rows of the accession table
```
## DELETE FROM lis_germplasm.grin_accession;
```

Run the R script `brapi-to-postgresql.R` to populate the accession table from the BrAPI endpoint.
Then check the new row count,
```
## SELECT COUNT(*) FROM lis_germplasm.grin_accession;
145629
```

Done! Exit psql/postgres
```
## \q
# exit
```

Dump the modified database (uncompressed and compressed versions)
```
$ pg_dump lis_germplasm > lis_germplasm.sql
$ pg_dump lis_germplasm --no-owner --no-privileges --compress=9 > lis_germplasm.sql.gz
```

Copy the compressed dump to the right place
```
$ cp -p lis_germplasm.sql.gz [...]/lis_gis/docker-entrypoint-initdb.d/.
```

Rebuild the Docker container, having first commented out line 2 of `Dockerfile` to prevent it from creating the default `lis_germplasm.sql.xz`
```
$ cd [...]/lis_gis
$ sudo docker compose down
$ sudo docker compose up -d --wait --build
```

Compare to original at https://germplasm-map.legumeinfo.org

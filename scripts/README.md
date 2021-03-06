
Data curation scripts
=====================

Adding NA values to DATA files
------------------------------

To fill in NA values for all societies in a dataset which have no row for a variable in
the corresponding `DATA` file, run

```bash
cd scripts
python add_na.py
```

This script is idempotent, i.e. running it for a second time should result in

```
$ python add_na.py 
EA 1291 societies
0 NA values added
Binford 339 societies
0 NA values added
```


Data from Glottolog
-------------------

When [Glottolog](http://glottolog.org) releases a new version, the following steps have
to be taken to recreate the dplace data which is derived from Glottolog:

1. Recreate the NEXUS files for Glottolog top-level family trees running
   ```
   python glottolog.py trees
   ```

2. Recreate the languoid data in `csv/glottolog.csv` by running the following command
   on a PostgreSQL database with the Glottolog SQL dump loaded:
   ```
   python glottolog.py languoids postgresql://dbuser@/glottolog
   ```


Precomputed society-region mapping
----------------------------------

Require `csvkit`, `fiona` and `shapely`, all of which can be installed with `pip`.

1. Compute mapping:
   ```
   cd scripts
   python geo.py
   ```

2. Merge region-mapping and `csv/LatLong_data.csv` into `csv/society_locations.csv`.
   ```
   cd ../csv
   csvjoin LatLong_data.csv society_region.csv -c soc_id | csvcut -C 8  > society_locations.csv
   rm society_region.csv
   ```


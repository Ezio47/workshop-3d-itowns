# Districts

We will now import the districts of the city, stored in a GeoJSON file, into our database.

The districts are represented geometrically by a 2D polygon.

## Importing the data

The ogr2ogr program allows us to load all of the data contained in the GeoJSON file in our database.

`ogr2ogr -f "PostgreSQL" PG:"dbname=gml" "/home/oslandia/data/montreal/geojson/district.json" -nln <district table> -append`

We can check the result:

```
psql gml
SELECT num, nom FROM <district table>;
```

You should see 34 districts in the table.

## Transforming the data

The district data is in the geographic coordinate system. We will project it in our local srs: epsg:2950.

```
psql gml
SELECT AddGeometryColumn('<district table>', 'geom', 2950, 'MULTIPOLYGON', 2);
UPDATE <district table> SET geom = ST_TRANSFORM(wkb_geometry, 2950);
```
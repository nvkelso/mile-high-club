![](http://github.com/nvkelso/mile-high-club/raw/master/geo-data/images/800px-Airport_infrastructure.png)

The "location" column is based on the above Airport infrastructure diagram (Source: [Wikipedia](http://en.wikipedia.org/wiki/File:Airport_infrastructure.png)).

## Location values

_Ordered best to worst_

**Terminal** - Passenger terminal (not freight, not commercial or private pilot hangers). 

If there is more than one terminal building, the International Terminal, followed by 
Terminal 1, or the largest/central building. If multiple terminals are dispersed, the 
centroid between them is used (often in a parking lot or round about area). See London 
Heathrow for example. The military terminal is not preferred at joint civilian airports. 

**Parking** - If terminals are spread out, use the centroid between them. Most often a parking lot.

**Ramp** - Pavement area outside the passenger terminal area. Used when terminal building is not clear 
on airphoto interpretation.

**Runway** - Center of the runway. If more than one runway, the centroid. Some of these need 
nudging over to the centroid. Can include taxiways if difficult to distinguish between 
runway and taxiway on airphoto interpretation.

**Approximate** - Could be up to 20 miles away from the actual airport location. 
More often it's within a mile of the actual location. Accuracy is low due to geocoding 
based on town name, use of DD.DD° or DD.DDDD° format instead of DD.DDDDDD° for latitude 
and longitude values.

## Exporting for Natural Earth

The included shell script:

	export_natural_earth_version.sh
	
can be run as:

	./export_natural_earth_version.sh
	
To create the following files:

	ne_10m_airports.dbf
	ne_10m_airports.prj
	ne_10m_airports.shp
	ne_10m_airports.shx
	
Using the following **ogr2ogr** command (relies on GDAL and OGR being installed and will **overwrite** existing files):

	ogr2ogr -overwrite -sql "SELECT scaleRank as scalerank, 'Airport' as featurecla, type_nvk as type, label_lng as name, label_sm as abbrev, location, gps_code, iata_cod_1 as iata_code, wikipedia_ as wikipedia, natlScale FROM aaron_airports_oct26_win1252_plus_dafif_plus_our_all WHERE natlScale >= 8 ORDER BY natlScale" ne_10m_airports.shp aaron_airports_oct26_win1252_plus_dafif_plus_our_all.shp
	
Note: 
	
**natlScale** is converted to Natural Earth's modified web map zoom integers (**scaleRank**):

    Conversion from: https://github.com/nvkelso/geo-how-to/wiki/Map-scales---zooms
    Which are slightly differnt than raw web map zooms.
    
	if [natlScale] = 150     then scaleRank = 2    // consistent with web zoom...
	if [natlScale] =  75     then scaleRank = 3    
	if [natlScale] =  50     then scaleRank = 4    // special to natural earth
	if [natlScale] =  30     then scaleRank = 5    // 1 off from web zoom...
	if [natlScale] =  20     then scaleRank = 6
	if [natlScale] =  15     then scaleRank = 7    // this could use some work, edit out
	if [natlScale] =  10     then scaleRank = 8
	if [natlScale] =   8     then scaleRank = 9    // note 5 and 8 merge
	if [natlScale] =   5     then scaleRank = 9    // note 5 and 8 merge
	if [natlScale] =   2     then scaleRank = 10
	if [natlScale] =   1     then scaleRank = 11
	if [natlScale] =   0.5   then scaleRank = 12
	if [natlScale] =   0.25  then scaleRank = 13
	if [natlScale] =   0.15  then scaleRank = 14
	if [natlScale] =   0.10  then scaleRank = 15
	if [natlScale] =   0.09  then scaleRank = 16   // what is this doing here?
	if [natlScale] =   0.015 then scaleRank = 17   // paragliders
	if [natlScale] =   0.01  then scaleRank = 18   // mostly back on track
	if [natlScale] =  -1     then scaleRank = 100  // these should be removed

TODO:

1. Include the fullname of the airport (now it's only the name with abbreviated Int'l, etc).

## Metadata

These GIS data files are in Windows-1252 character encoding and in the geographic lat/long projection (WGS84). 

They are from a variety of sources, merged and quality checked by Nathaniel Vaughn KELSO based partly on work by Aaron.

## Requirements 

Only for shell script mentioned above only.

* GDAL/OGR => `1.9`
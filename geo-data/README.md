![](http://github.com/nvkelso/mile-high-club/raw/master/geo-data/images/800px-Airport_infrastructure.png)

The "location" column is based on the above Airport infrastructure diagram (Source: [Wikipedia](http://en.wikipedia.org/wiki/File:Airport_infrastructure.png)).

## Location values

_Ordered best to worst_

**Terminal** - Passenger terminal (not freight, not commercial or private pilot hangers). 

If there is more than one terminal building, the International Terminal, followed by 
Terminal 1, or the largest/central building. If multiple terminals are dispersed, the 
centroid between them is used (often in a parking lot or round about area). See London 
Heathrow for example. The military terminal is not preferred at joint civilian airports. 

**Ramp** - Pavement area outside the passenger terminal area. Used when terminal building is not clear 
on airphoto interpretation.

**Runway** - Center of the runway. If more than one runway, the centroid. Some of these need 
nudging over to the centroid. Can include taxiways if difficult to distinguish between 
runway and taxiway on airphoto interpretation.

**Approximate** - Could be up to 20 miles away from the actual airport location. 
More often it's within a mile of the actual location. Accuracy is low due to geocoding 
based on town name, use of DD.DD° or DD.DDDD° format instead of DD.DDDDDD° for latitude 
and longitude values.
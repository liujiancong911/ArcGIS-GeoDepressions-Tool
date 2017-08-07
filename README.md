# ArcGIS-GeoDepressions-Tool

An ArcGIS toolbox to semi-automatically identify and analyse geo-depressions in a bathymetric grid. These tools can be used to aid the discovery of pockmarks on the sea bed, and ultimately the location of working hydrocarbon systems.

Pockmarks, or seafloor depressions are often associated with fluid discharge and are regarded as indicators of focused fluid seepage. First detailed by King & Maclean (1970); they suggest that gas from underlying bedrock was released sufficiently enough to put fine grain material into suspension. As this sediment drifts away from the venting point is results in a depression in the sea floor. The shape and size of pockmarks can vary greatly, however there are some relatively common traits. Hovland et al (2002) devised several morphological categories. The most common are:
* A unit pockmark (1-10m in diameter, up to 0.5m deep)
* 'Normal' pockmarks (10-700m in diameter, up to 45m deep)
* Elongate pockmarks (Similar to normal pockmarks except one axis is considerably longer)

## Identify GeoDepression Tool

Identifies depressions in a bathymetric raster using the specified z-value (difference between sink and pour point). The z-value parameter is a threshold for what sinks should be filled in the raster. A sink is a cell in the raster that has no outward flow direction (i.e. a depression). If the sink depth to pour point height is less than the specified z-value, the sink will be identified; otherwise it will be ignored. Ideally this tool will be run several times with differing z-values to capture all possible depressions. For e.g. depressions that are ~5m deep would be ignored if they were located inside a depression that is ~20m deep if the z-value was >20m.

### Algorithm:

1. Use ArcGIS Spatial Analysis Fill tool to fill all sinks within z-value range
1. Subtract newly created fill raster from step 1 from original bathymetry raster layer. Resulting layer is a raster of depressions
1. Reclassify depressions raster and convert to polygons
1. Calculate area and remove all depression polygons that fall outside of the specified range. The minimum area is pre-defined as (cellsize*3)² to allow enough raster resolution to delineate a shape
1. Use ArcGIS Spatial Analysis Zonal Statistics tool to find the deepest point in each depression
1. Spatially join the deepest point to the depression polygons

## Analyse GeoDepressions Tool



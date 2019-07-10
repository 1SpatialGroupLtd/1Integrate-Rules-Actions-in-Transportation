# Essential Geometry Checks
This repo folder contains the Essential Geometry Checks [Ruleset](Essential%20Geometry%20Checks.rules) that can be used within [1Integrate for ArcGIS](https://1spatial.com/products/1integrate-for-arcgis/).  
For instructions on uploading and publishing Rulesets, please refer to the following [documentation](https://1spatial.com/documentation/1integrate-arcgis/v2/Topics/Rules/Free_Rulesets.htm)

## Check for Duplicate Features
Checks for two features within the same class (aka Feature Class) that have the same geometry.

### Rule Syntax
Check for objects ALL that there are no objects ALL2 for which :ALL2.geometry equals :ALL.geometry and class(:ALL2) equals class(:ALL) and :ALL2 does not equal :ALL.  
![Alt text](img/DuplicateFeaturesRule.png?raw=true "Duplicate Feature Rule Screenshot")

### Report Action Syntax
Reports on a feature that has a duplicate geometry.  
Action for all objects ALL: if it is the case that :ALL.geometry is not equal to null then let dupfeatgeom = :ALL.geometry and then report on get_point(dupfeatgeom).  
![Alt text](img/DuplicateFeaturesReportAction.png?raw=true "Duplicate Feature Report Action Screenshot")


## Check for Duplicate Vertices
Checks that a geometry (Line or Polygon) does not have any consecutive coincident vertices. This rule uses the built-in function has_duplicates() that tests to see if a geometry has any consecutive coincident vertices.  It returns a Boolean value, 'true' if the geometry has any consecutive coincident vertices and 'false' if it does not.  

### Rule Syntax
Check for objects ALL that has_duplicates(:ALL.geometry) equals 'false'.  
![Alt text](img/DuplicateVerticesRule.png?raw=true "Duplicate Vertex Rule Screenshot")

### Report Action Syntax
Reports on each vertex that has a duplicate as defined above.  
Action for all objects ALL: if it is the case that :ALL.geometry is not equal to null then let dupPointsCollection = geom_find_duplicates(:ALL.geometry, 0) and then for all objects duppoint in dupPointsCollection do report on duppoint.  
![Alt text](img/DuplicateVerticesReportAction.png?raw=true "Duplicate Vertex Action Screenshot")


## Check for Kickbacks
Checks whether a geometry has any kickbacks (a.k.a. snap-backs, cutbacks). A kickback is a type of geometric error where a line segment changes direction twice by approximately 180 degrees to repeat part of the line.  This rule uses the built-in function has_kickbacks(). The parameters can be configured. The inputs are 1) geometry to test. 2) (optional) The maximum value for the sine of the angles in the kickback. Note: If omitted, this defaults to the sine of 1 degree. 3) (optional) The maximum width of the kickback.  
This rule uses a 12 degree tolerance.  
![Alt text](img/KickbackExample.png?raw=true "Kickback Example")

### Rule Syntax
Check for objects ALL that has_kickbacks(:ALL.geometry,sin(to_radians(12))) equals 'false'.  
![Alt text](img/KickbackRule.png?raw=true "Kickback Rule Screenshot")

### Report Action Syntax
Reports on each instance of a kickback on the non-conforming geometry. The reporting point location is within the center of the actual kickback.  
Action for all objects ALL: if it is the case that :ALL.geometry is not equal to null then let kickbackgeom = geom_find_kickbacks(:ALL.geometry, sin(to_radians(12))) and then for all objects eachpart in kickbackgeom do report on eachpart.  
![Alt text](img/KickbackReportAction.png?raw=true "Kickback Report Action Screenshot")


## Check OGC Simple Feature
Tests to see if a feature is simple based on the [OGC definition](http://www.opengeospatial.org/standards/sfa).  
According to the OGC Specifications, a simple geometry is one that has no anomalous geometric points, such as self intersection or self tangency and primarily refers to 0 or 1-dimensional geometries (i.e. [MULTI]POINT, [MULTI]LINESTRING). Geometry validity, on the other hand, primarily refers to 2-dimensional geometries (i.e. [MULTI]POLYGON) and defines the set of assertions that characterizes a valid polygon. The description of each geometric class includes specific conditions that further detail geometric simplicity and validity.
A POINT is inheritably simple as a 0-dimensional geometry object.  
MULTIPOINTs are simple if no two coordinates (POINTs) are equal (have identical coordinate values).  
A LINESTRING is simple if it does not pass through the same POINT twice (except for the endpoints, in which case it is referred to as a linear ring and additionally considered closed).  
The information on OGC Simple was pulled from the PostGIS Documentation on [OGC Simplicity](https://postgis.net/docs/using_postgis_dbmanagement.html#OGC_Validity).

### Rule Syntax
Check for all objects ALL that is_simple(:ALL.geometry) is equal to 'true'.  
![Alt text](img/CheckOGCSimpleRule.png?raw=true "OGC Simple Rule Screenshot")

### Report Action Syntax
Reports on each feature that was reported as a non-conformance from the Check OGC Simple Feature rule.  
NOTE: Must be paired as a reporting action to the rule above to report correctly.  
Action for all objects ALL: let issimplegeom = :ALL.geometry and then report on get_point(issimplegeom).  
![Alt text](img/CheckOGCSimpleReportAction.png?raw=true "Kickback Report Action Screenshot")


## Check OGC Valid Feature
Tests to see if a feature is valid based on the [OGC definition](http://www.opengeospatial.org/standards/sfa).  
By definition, a POLYGON is always simple. It is valid if no two rings in the boundary (made up of an exterior ring and interior rings) cross. The boundary of a POLYGON may intersect at a POINT but only as a tangent (i.e. not on a line). A POLYGON may not have cut lines or spikes and the interior rings must be contained entirely within the exterior ring.  
A MULTIPOLYGON is valid if and only if all of its elements are valid and the interiors of no two elements intersect. The boundaries of any two elements may touch, but only at a finite number of POINTs.  
The information on OGC Simple and OGC Valid was pulled from the PostGIS Documentation on [OGC Validity](https://postgis.net/docs/using_postgis_dbmanagement.html#OGC_Validity).  

### Rule Syntax
Check for all objects ALL that is_valid(:ALL.geometry) is equal to 'true'.  
![Alt text](img/CheckOGCValidRule.png?raw=true "OGC Valid Rule Screenshot")

### Report Action Syntax
Reports on each feature that was reported as a non-conformance from the Check OGC Valid Feature rule.  
NOTE: Must be paired as a reporting action to the rule above to report correctly.  
Action for all objects ALL: let isvalidgeom = :ALL.geometry and then report on get_point(isvalidgeom).  
![Alt text](img/CheckOGCValidReportAction.png?raw=true "Kickback Report Action Screenshot")


## Check Self-Intersections
Checks polylines and polygons for self-intersections. Uses the built-in function find_self_intersections() which returns a multipart geometry representing each point location where self-intersections occur.  

### Rule Syntax
Check for all objects ALL that: if it is the case that (geom_get_type(:ALL.geometry) is equal to 1 or geom_get_type(:ALL.geometry) is equal to  2) then (geom_find_self_intersections(:ALL.geometry) is not equal to null and count_parts(geom_find_self_intersections(:ALL.geometry)) is greater than 0).  
![Alt text](img/CheckSelf-IntersectionRule.png?raw=true "Self-Intersection Rule Screenshot")

### Report Action Syntax
Reports on the exact location of each self-intersecting polyline or polygon. Uses the built-in function find_self_intersections() which returns a multipart geometry representing each point location where self-intersections occur.  
Action for all objects ALL: if it is the case that :ALL.geometry is not equal to null then let selfintersect = geom_find_self_intersections(:ALL.geometry) and then for all objects eachpart in selfintersect do report on eachpart.  
![Alt text](img/CheckSelf-IntersectionReportAction.png?raw=true "Kickback Report Action Screenshot")


## Check for Single Part Geometries
Checks that geometries are single part and not a multi-part geometry. Multi-part geometries are geometries that contain more than one part. For example, the State of Hawaii, if modeled as one feature in a table, would be a multi-part polygon because it contains multiple polygon geometries for each island within that record.  This rule uses the Count-Parts to determine if there is only 1 part for the geometry.  If the feature has no geometry or has more than 1 part, it will be flagged as a non-conformance.  
![Alt text](img/MultiPartExample.png?raw=true "Multipart Example in Hawaii")

### Rule Syntax
Check for objects ALL that count_parts(:ALL.geometry) equals 1.  
![Alt text](img/CountPartsRule.png?raw=true "Multi-Part Rule Screenshot")

### Report Action Syntax
Reports on each part of a geometry if the geometry fails the Check Single Part Geometry Check rule.  
NOTE: Must be paired as a reporting action to the rule above to report correctly.  
Action for all objects ALL: let simplegeom = :ALL.geometry and then for all objects eachpart in simplegeom do report on get_point(eachpart).  
![Alt text](img/CountPartsReportAction.png?raw=true "Multi-Part Rule Screenshot")


## Check for Spikes
Checks whether a geometry has any spikes. A spike is defined to be three consecutive points (A, B, C) such that: 1) The distance AB is less than the distance BC. 2) The sine of the angle ABC is less than a maximum value which may be specified by the second parameter. 3) (Optionally) the distance AB is less than a maximum "length" value specified by the third parameter.  This rule uses the has_spikes() built-in function which has the following parameters.  1) The geometry to test. 2) (optional) The maximum value for the sine of the angle in the spike (a real number in the range [0, 1]). Note: If omitted, this defaults to the sine of 1 degree (approximately 0.017). 3) (optional) The maximum length of the spike.  
This rule uses a 6 degree tolerance.  
![Alt text](img/SpikeExample.png?raw=true "Spike Example")

### Rule Syntax
Check for objects ALL that has_spikes(:ALL.geometry,sin(to_radians(6))) equals 'false'.  
![Alt text](img/SpikeRule.png?raw=true "Spike Rule Screenshot")

### Report Action Syntax
Reports on each instance of a spike on the non-conforming geometry. The reporting point location is at the exact location (the vertex) causing the spike in the geometry.  
Action for all objects ALL: if it is the case that :ALL.geometry is not equal to null then let spikegeom = find_spikes(:ALL.geometry, sin(to_radians(6))) and then for all objects eachpart in spikegeom do report on eachpart.  
![Alt text](img/SpikeReportAction.png?raw=true "Spike Report Action Screenshot")


## Licensing
Copyright © 2018 1Spatial US Patent Number 9542416 B2 (2017-01-10)

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

   http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

A copy of the license is available in the repository's [license.txt](LICENSE) file.

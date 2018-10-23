# Essential Geometry Checks
This repo folder contains the Essential Geometry Checks by Rule that can be used within 1Integrate.  
For instructions on restoring a backup please refer to the following [documentation](https://1spatial.com/documentation/1integrate/v2_3/Topics/Backup_Restore.htm?Highlight=Restore%20Backup%20Rules)

## [Check for Duplicate Features](CheckForDuplicateFeatures.xml)
Checks for two features within the same class (aka Feature Class) that has the same geometry 
### Rule Syntax
Check for objects all that there are no objects duplicate features for which :duplicate features.geometry equals :all.geometry and class(:duplicate features) equals class(:all) and :duplicate features does not equal :all  
![Alt text](img/DuplicateFeaturesRule.png?raw=true "Duplicate Feature Rule Screenshot")

## [Check for Duplicate Vertices](CheckForDuplicateVertices.xml)
Checks for Duplicate Verticies.  The Duplicate Vertex checks a geometry (Line or Polygon) that has any consecutive coincident vertices.  This rule uses the Built-In function has_duplicates() that Tests to see if a geometry has any consecutive coincident vertices.  It returns a Boolean value, true if the geometry has any consecutive coincident vertices and false if it does not. 
### Rule Syntax
Check for objects all that has_duplicates(:all.geometry) equals false  
![Alt text](img/DuplicateVerticesRule.png?raw=true "Duplicate Vertex Rule Screenshot")

## [Check for Kickbacks](CheckForKickbacks.xml)
Checks whether a geometry has any kickbacks, aka snap-backs, cutbacks, (a type of geometric error where a line segment changes direction twice by approximately 180 degrees to repeat part of the line).  This rule uses the has_kickbacks built-in function.  The has_kickbacks function Checks whether a geometry has any kickbacks (a type of geometric error where a line segment changes direction twice by approximately 180 degrees to repeat part of the line).  The parameters can be configured.  The inputs are 1) geometry to test. 2) (optional) The maximum value for the sine of the angles in the kickback. Note: If omitted, this defaults to the sine of 1 degree. 3) (optional) The maximum width of the kickback. 
This rule uses a 12 degree tolerance.  
![Alt text](img/KickbackExample.png?raw=true "Kickback Example")

### Rule Syntax
Check for objects all that has_kickbacks(:all.geometry,sin(to_radians(12))) equals false  
![Alt text](img/KickbackRule.png?raw=true "Kickback Rule Screenshot")

## [Check for Multipart Geometries](CheckForMultipartGeometries.xml)
Checks for Multi-Part Geometries.  A multi-part geometry are geometries that contain more than one part.  For example, the State of Hawaii, if modeled as one feature in a table, would be a multi-part polygon because it contains multiple polygon geometries for each island within that record.  This rule uses the Count-Parts to determine if there is only 1 part for the geometry.  If the feature has no geometry or has more than 1 part, it will be flagged as a non-conformance.
![Alt text](img/MultiPartExample.png?raw=true "Multipart Example in Hawaii")

### Rule Syntax
Check for objects all that count_parts(:all.geometry) equals 1  
![Alt text](img/CountPartsRule.png?raw=true "Multi-Part Rule Screenshot")

## [Check for OGC Simple](CheckForKickbacks.xml)
Tests to see if a feature is simple based on the [OGC definition](http://www.opengeospatial.org/standards/sfa).
According to the OGC Specifications, a simple geometry is one that has no anomalous geometric points, such as self intersection or self tangency and primarily refers to 0 or 1-dimensional geometries (i.e. [MULTI]POINT, [MULTI]LINESTRING). Geometry validity, on the other hand, primarily refers to 2-dimensional geometries (i.e. [MULTI]POLYGON) and defines the set of assertions that characterizes a valid polygon. The description of each geometric class includes specific conditions that further detail geometric simplicity and validity.
A POINT is inheritably simple as a 0-dimensional geometry object.
MULTIPOINTs are simple if no two coordinates (POINTs) are equal (have identical coordinate values).
A LINESTRING is simple if it does not pass through the same POINT twice (except for the endpoints, in which case it is referred to as a linear ring and additionally considered closed). 
The information on OGC Simple was pulled from the PostGIS Documentation on [OGC Validity](https://postgis.net/docs/using_postgis_dbmanagement.html#OGC_Validity).  

### Rule Syntax
Check for all objects ALL that: is_simple(ALL.geometry) equals true  
![Alt text](img/OGCSimpleRule.png?raw=true "OGC Simple Rule Screenshot")

## Check for OGC Valid
Tests to see if a feature is valid based on the [OGC definition](http://www.opengeospatial.org/standards/sfa).
By definition, a POLYGON is always simple. It is valid if no two rings in the boundary (made up of an exterior ring and interior rings) cross. The boundary of a POLYGON may intersect at a POINT but only as a tangent (i.e. not on a line). A POLYGON may not have cut lines or spikes and the interior rings must be contained entirely within the exterior ring.
A MULTIPOLYGON is valid if and only if all of its elements are valid and the interiors of no two elements intersect. The boundaries of any two elements may touch, but only at a finite number of POINTs.
The information on OGC Simple and OGC Valid was pulled from the PostGIS Documentation on [OGC Validity](https://postgis.net/docs/using_postgis_dbmanagement.html#OGC_Validity).  

### Rule Syntax
Check for all objects ALL that: is_valid(ALL.geometry) equals true  
![Alt text](img/OGCValidRule.png?raw=true "OGC Valid Rule Screenshot")

## [Check for Spikes](CheckForSpikes.xml)
Checks whether a geometry has any spikes.  A spike is defined to be three consecutive points (A, B, C) such that: 1) The distance AB is less than the distance BC. 2) The sine of the angle ABC is less than a maximum value which may be specified by the second parameter. 3) (Optionally) the distance AB is less than a maximum "length" value specified by the third parameter.  This rule uses the has_spikes() built-in function which has the following parameters.  1) The geometry to test. 2) (optional) The maximum value for the sine of the angle in the spike (a real number in the range [0, 1]). Note: If omitted, this defaults to the sine of 1 degree (approximately 0.017). 3) (optional) The maximum length of the spike.
This rule uses a 6 degree tolerance.  
![Alt text](img/SpikeExample.png?raw=true "Spike Example")

### Rule Syntax
Check for objects all that has_spikes(:all.geometry,sin(to_radians(6))) equals false  
![Alt text](img/SpikeRule.png?raw=true "Spike Rule Screenshot")

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
# Essential Geometry Checks
This repo folder contains the Essential Geometry Checks [Ruleset](Essential%20Geometry%20Checks.rules) that can be used within [1Integrate for ArcGIS](https://1spatial.com/products/1integrate-for-arcgis/).  
For instructions on uploading and publishing Rulesets, please refer to the following [documentation](https://1spatial.com/documentation/1integrate-arcgis/v2/Topics/Rules/Free_Rulesets.htm)

## Check for Duplicate Features
Checks for two features within the same class (aka Feature Class) that has the same geometry 
### Rule Syntax
Check for objects all that there are no objects duplicate features for which :duplicate features.geometry equals :all.geometry and class(:duplicate features) equals class(:all) and :duplicate features does not equal :all  
![Alt text](img/DuplicateFeaturesRule.png?raw=true "Duplicate Feature Rule Screenshot")
### Report Action Syntax
Reports on a feature that has a duplicate geometry.
Action for all objects ALL: if it is the case that ALL.geometry is not equal to null then let dupfeatgeom = ALL.geometry and then report on get_point(dupfeatgeom)
![Alt text](img/DuplicateFeaturesReportAction.png?raw=true "Duplicate Feature Report Action Screenshot")

## Check for Duplicate Vertices
Checks for Duplicate Verticies.  The Duplicate Vertex checks a geometry (Line or Polygon) that has any consecutive coincident vertices.  This rule uses the Built-In function has_duplicates() that Tests to see if a geometry has any consecutive coincident vertices.  It returns a Boolean value, true if the geometry has any consecutive coincident vertices and false if it does not. 
### Rule Syntax
Check for objects all that has_duplicates(:all.geometry) equals false  
![Alt text](img/DuplicateVerticesRule.png?raw=true "Duplicate Vertex Rule Screenshot")
### Report Action Syntax
Reports on each vertex that has a duplicate as defined above.
Action for all objects ALL: if it is the case that ALL.geometry is not equal to null then let dupPointsCollection = geom_find_duplicates(ALL.geometry, 0) and then for all objects duppoint in dupPointsCollection do report on duppoint  
![Alt text](img/DuplicateVerticesReportAction.png?raw=true "Duplicate Vertex Action Screenshot")

## Check for Kickbacks
Checks whether a geometry has any kickbacks, aka snap-backs, cutbacks, (a type of geometric error where a line segment changes direction twice by approximately 180 degrees to repeat part of the line).  This rule uses the has_kickbacks built-in function.  The has_kickbacks function Checks whether a geometry has any kickbacks (a type of geometric error where a line segment changes direction twice by approximately 180 degrees to repeat part of the line).  The parameters can be configured.  The inputs are 1) geometry to test. 2) (optional) The maximum value for the sine of the angles in the kickback. Note: If omitted, this defaults to the sine of 1 degree. 3) (optional) The maximum width of the kickback. 
This rule uses a 12 degree tolerance.  
![Alt text](img/KickbackExample.png?raw=true "Kickback Example")

### Rule Syntax
Check for objects all that has_kickbacks(:all.geometry,sin(to_radians(12))) equals false  
![Alt text](img/KickbackRule.png?raw=true "Kickback Rule Screenshot")

### Report Action Syntax
Reports on each instance of a kickback on the non-conforming geometry.  The reporting point location is within the center of the actual kickback.
Action for all objects ALL: if it is the case that ALL.geometry is not equal to null then let kickbackgeom = geom_find_kickbacks(ALL.geometry, sin(to_radians(12))) and then for all objects eachpart in kickbackgeom do report on eachpart  
![Alt text](img/KickbackReportAction.png?raw=true "Kickback Report Action Screenshot")

## Check for Multipart Geometries
Checks for Multi-Part Geometries.  A multi-part geometry are geometries that contain more than one part.  For example, the State of Hawaii, if modeled as one feature in a table, would be a multi-part polygon because it contains multiple polygon geometries for each island within that record.  This rule uses the Count-Parts to determine if there is only 1 part for the geometry.  If the feature has no geometry or has more than 1 part, it will be flagged as a non-conformance.
![Alt text](img/MultiPartExample.png?raw=true "Multipart Example in Hawaii")

### Rule Syntax
Check for objects all that count_parts(:all.geometry) equals 1  
![Alt text](img/CountPartsRule.png?raw=true "Multi-Part Rule Screenshot")

## Check for Spikes
Checks whether a geometry has any spikes.  A spike is defined to be three consecutive points (A, B, C) such that: 1) The distance AB is less than the distance BC. 2) The sine of the angle ABC is less than a maximum value which may be specified by the second parameter. 3) (Optionally) the distance AB is less than a maximum "length" value specified by the third parameter.  This rule uses the has_spikes() built-in function which has the following parameters.  1) The geometry to test. 2) (optional) The maximum value for the sine of the angle in the spike (a real number in the range [0, 1]). Note: If omitted, this defaults to the sine of 1 degree (approximately 0.017). 3) (optional) The maximum length of the spike.
This rule uses a 6 degree tolerance.  
![Alt text](img/SpikeExample.png?raw=true "Spike Example")

### Rule Syntax
Check for objects all that has_spikes(:all.geometry,sin(to_radians(6))) equals false  
![Alt text](img/SpikeRule.png?raw=true "Spike Rule Screenshot")

### Report Action Syntax
Reports on each instance of a spike on the non-conforming geometry.  The reporting point location is at the exact location (the vertex) causing the spike in the geometry.
Action for all objects ALL: if it is the case that ALL.geometry is not equal to null then let spikegeom = find_spikes(ALL.geometry, sin(to_radians(6))) and then for all objects eachpart in spikegeom do report on eachpart  
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

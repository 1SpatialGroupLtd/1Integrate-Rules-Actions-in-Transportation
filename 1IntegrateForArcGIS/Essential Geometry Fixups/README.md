# Essential Geometry Fixups
This repo folder contains the Essential Geometry Fixups [Ruleset](Essential%20Geometry%20Fixups.rules) that can be used within [1Integrate for ArcGIS](https://1spatial.com/products/1integrate-for-arcgis/).  
For instructions on uploading and publishing Rulesets, please refer to the following [documentation](https://1spatial.com/documentation/1integrate-arcgis/v2/Topics/Rules/Free_Rulesets.htm)  

This Ruleset is organized with Rules and Cleanup Actions.  Each rule associates with a cleanup action.  If a feature does not conform to the rule, then that feature is passed to the action.


## Fix Duplicate Features
Identifies duplicate features (Checks for two features within the same class (aka Feature Class) that have the same geometry) and then deletes each duplicate.  NOTE: This action is configured to only look at duplicate geometries.  The action can be configured to also look at duplicate geometries and attributes. 

### Rule Syntax
Check for objects ALL that there are no objects ALL2 for which :ALL2.geometry equals :ALL.geometry and class(:ALL2) equals class(:ALL) and :ALL2 does not equal :ALL.  
![Alt text](img/DuplicateFeaturesRule.png?raw=true "Duplicate Feature Rule Screenshot")

### Cleanup Action Syntax
Identifies duplicate features (Checks for two features within the same class (aka Feature Class) that have the same geometry) and then deletes each duplicate.  NOTE: This action is configured to only look at duplicate geometries.  The action can be configured to also look at duplicate geometries and attributes. 

Action for all objects ALL: if it is the case that there is at least 1 object ALL2 for which (:ALL.geometry spatially equals :ALL2.geometry and :ALL is not equal to :ALL2 and class(:ALL) is equal to class(:ALL2)) then delete object :ALL.  
![Alt text](img/DuplicateFeaturesFixupAction.png?raw=true "Duplicate Feature Fixup Action Screenshot")


## Fix Duplicate Vertices
Identifies and then deletes all duplicate vertices.  The rule checks for a geometry (Line or Polygon) that has any consecutive coincident vertices.  This rule uses the built-in function has_duplicates() that tests to see if a geometry has any consecutive coincident vertices.  It returns a Boolean value, 'true' if the geometry has any consecutive coincident vertices and 'false' if it does not.  Then the built-in function remove_duplicates() removes all duplicate verticies within a geometry.

### Rule Syntax
Check for objects ALL that has_duplicates(:ALL.geometry) equals 'false'.  
![Alt text](img/DuplicateVerticesRule.png?raw=true "Duplicate Vertex Rule Screenshot")

### Cleanup Action Syntax
Identifies and then deletes all duplicate vertices.  The rule checks for a geometry (Line or Polygon) that has any consecutive coincident vertices.  This rule uses the built-in function has_duplicates() that tests to see if a geometry has any consecutive coincident vertices.  It returns a Boolean value, 'true' if the geometry has any consecutive coincident vertices and 'false' if it does not.  Then the built-in function remove_duplicates() removes all duplicate verticies within a geometry.

Action for all objects ALL: if it is the case that has_duplicates(:ALL.geometry) is equal to  true then let :ALL.geometry = remove_duplicates(:ALL.geometry).  
![Alt text](img/DuplicateVerticesFixupAction.png?raw=true "Duplicate Vertex Fixup Action Screenshot")


## Fix Kickbacks
Identifies features that has kickbacks and then removes each kickback by deleting the vertices that create the kickback. A kickback (a.k.a. snap-backs, cutbacks) is a type of geometric error where a line segment changes direction twice by approximately 180 degrees to repeat part of the line.  This rule uses the built-in function has_kickbacks(). The parameters can be configured. The inputs are 1) geometry to test. 2) (optional) The maximum value for the sine of the angles in the kickback. Note: If omitted, this defaults to the sine of 1 degree. 3) (optional) The maximum width of the kickback. It also runs the remove_kickbacks() built-in function which fixes the kickbacks and uses the same parameters as the find_kickback() built-in.  
This rule uses a 12 degree tolerance.  
![Alt text](img/KickbackFix.png?raw=true "Kickback Fixup Example")

### Rule Syntax
Checks whether a geometry has any kickbacks (a.k.a. snap-backs, cutbacks). A kickback is a type of geometric error where a line segment changes direction twice by approximately 180 degrees to repeat part of the line.  This rule uses the built-in function has_kickbacks(). The parameters can be configured. The inputs are 1) geometry to test. 2) (optional) The maximum value for the sine of the angles in the kickback. Note: If omitted, this defaults to the sine of 1 degree. 3) (optional) The maximum width of the kickback.  
This rule uses a 12 degree tolerance.  

Check for objects ALL that has_kickbacks(:ALL.geometry,sin(to_radians(12))) equals 'false'.  
![Alt text](img/KickbackRule.png?raw=true "Kickback Rule Screenshot")

### Cleanup Action Syntax
Identifies features that has kickbacks and then removes each kickback by deleting the vertices that create the kickback. A kickback (a.k.a. snap-backs, cutbacks) is a type of geometric error where a line segment changes direction twice by approximately 180 degrees to repeat part of the line.  This rule uses the built-in function has_kickbacks(). The parameters can be configured. The inputs are 1) geometry to test. 2) (optional) The maximum value for the sine of the angles in the kickback. Note: If omitted, this defaults to the sine of 1 degree. 3) (optional) The maximum width of the kickback. It also runs the remove_kickbacks() built-in function which fixes the kickbacks and uses the same parameters as the find_kickback() built-in.  

Action for all objects ALL: if it is the case that :ALL.geometry is not equal to null then let kickbackgeom = geom_find_kickbacks(:ALL.geometry, sin(to_radians(12))) and then for all objects eachpart in kickbackgeom do report on eachpart.  
![Alt text](img/KickbackCleanupAction.png?raw=true "Kickback Cleanup Action Screenshot")


## Fix Spikes
Identifies features that has spikes and then removes each spike by deleting the vertex that creates the spike.  A spike is defined to be three consecutive points (A, B, C) such that: 1) The distance AB is less than the distance BC. 2) The sine of the angle ABC is less than a maximum value which may be specified by the second parameter. 3) (Optionally) the distance AB is less than a maximum "length" value specified by the third parameter.  This rule uses the has_spikes() built-in function which has the following parameters.  1) The geometry to test. 2) (optional) The maximum value for the sine of the angle in the spike (a real number in the range [0, 1]). Note: If omitted, this defaults to the sine of 1 degree (approximately 0.017). 3) (optional) The maximum length of the spike.  The action also uses the remove_spikes() built-in which removes all spikes using the same parameters as the find_spikes() built-in.  
This rule uses a 6 degree tolerance.  

![Alt text](img/SpikeFix.png?raw=true "Spike Fixup Example")

### Rule Syntax
Checks whether a geometry has any spikes.  A spike is defined to be three consecutive points (A, B, C) such that: 1) The distance AB is less than the distance BC. 2) The sine of the angle ABC is less than a maximum value which may be specified by the second parameter. 3) (Optionally) the distance AB is less than a maximum "length" value specified by the third parameter.  This rule uses the has_spikes() built-in function which has the following parameters:  1) The geometry to test. 2) (optional) The maximum value for the sine of the angle in the spike (a real number in the range [0, 1]). Note: If omitted, this defaults to the sine of 1 degree (approximately 0.017). 3) (optional) The maximum length of the spike.  
This rule uses a 6 degree tolerance. 
 
Check for objects ALL that has_spikes(:ALL.geometry,sin(to_radians(6))) equals 'false'.  
![Alt text](img/SpikeRule.png?raw=true "Spike Rule Screenshot")

### Cleanup Action Syntax
Identifies features that has spikes and then removes each spike by deleting the vertex that creates the spike.  A spike is defined to be three consecutive points (A, B, C) such that: 1) The distance AB is less than the distance BC. 2) The sine of the angle ABC is less than a maximum value which may be specified by the second parameter. 3) (Optionally) the distance AB is less than a maximum "length" value specified by the third parameter.  This rule uses the has_spikes() built-in function which has the following parameters.  1) The geometry to test. 2) (optional) The maximum value for the sine of the angle in the spike (a real number in the range [0, 1]). Note: If omitted, this defaults to the sine of 1 degree (approximately 0.017). 3) (optional) The maximum length of the spike.  The action also uses the remove_spikes() built-in which removes all spikes using the same parameters as the find_spikes() built-in.  
This rule uses a 6 degree tolerance.  

Action for all objects ALL: if it is the case that has_spikes(:ALL.geometry, sin(to_radians(6))) is equal to  true then let :ALL.geometry = remove_spikes(:ALL.geometry, sin(to_radians(6))).  
![Alt text](img/SpikeCleanupAction.png?raw=true "Spike Cleanup Action Screenshot")


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
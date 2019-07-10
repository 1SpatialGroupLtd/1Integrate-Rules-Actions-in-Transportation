# Essential Geometry Fixups
This repo folder contains the Essential Geometry Fixups by Action that can be used within 1Integrate.  
For instructions on restoring a backup please refer to the following [documentation](https://1spatial.com/documentation/1integrate/v2_3/Topics/Backup_Restore.htm?Highlight=Restore%20Backup%20Rules)

## [Fix Duplicate Features](FixDuplicateFeatureAction.xml)
Identifies duplicate features (Checks for two features within the same class (aka Feature Class) that have the same geometry) and then deletes each duplicate.  NOTE: This action is configured to only look at duplicate geometries.  The action can be configured to also look at duplicate geometries and attributes. 

### Action Syntax
For objects ALL: if (there is at least 1 object ALL2 for which :ALL.geometry equals :ALL2.geometry and :ALL does not equal :ALL2 and class(:ALL) equals class(:ALL2)) then delete object :ALL  
![Alt text](img/RemoveDuplicateFeatureAction.png?raw=true "Remove Duplicate Features Action")

## [Fix Duplicate Vertices](FixDuplicateVerticiesAction.xml)
Identifies and then deletes all duplicate vertices.  The rule checks for a geometry (Line or Polygon) that has any consecutive coincident vertices.  This rule uses the built-in function has_duplicates() that tests to see if a geometry has any consecutive coincident vertices.  It returns a Boolean value, 'true' if the geometry has any consecutive coincident vertices and 'false' if it does not.  Then the built-in function remove_duplicates() removes all duplicate verticies within a geometry.

### Action Syntax
For objects ALL: if (has_duplicates(:ALL.geometry) equals true) then let :ALL.geometry = remove_duplicates(:ALL.geometry).  
![Alt text](img/RemoveDuplicateVertexAction.png?raw=true "Remove Duplicate Vertex Action")

## [Fix Kickbacks](FixKickbacksAction.xml)
Identifies features that has kickbacks and then removes each kickback by deleting the vertices that create the kickback. A kickback (a.k.a. snap-backs, cutbacks) is a type of geometric error where a line segment changes direction twice by approximately 180 degrees to repeat part of the line.  This rule uses the built-in function has_kickbacks(). The parameters can be configured. The inputs are 1) geometry to test. 2) (optional) The maximum value for the sine of the angles in the kickback. Note: If omitted, this defaults to the sine of 1 degree. 3) (optional) The maximum width of the kickback. It also runs the remove_kickbacks() built-in function which fixes the kickbacks and uses the same parameters as the find_kickback() built-in.  
This rule uses a 12 degree tolerance.  
![Alt text](img/KickbackFix.png?raw=true "Kickback Fixup Example")

### Action Syntax
For objects ALL: if (has_kickbacks(:ALL.geometry,sin(to_radians(12))) equals true) then let :ALL.geometry = remove_kickbacks(:ALL.geometry,sin(to_radians(12)))  
![Alt text](img/RemoveKickbacksAction.png?raw=true "Kickback Fix Action")


## [Fix Spikes](FixSpikesAction.xml)
Identifies features that has spikes and then removes each spike by deleting the vertex that creates the spike.  A spike is defined to be three consecutive points (A, B, C) such that: 1) The distance AB is less than the distance BC. 2) The sine of the angle ABC is less than a maximum value which may be specified by the second parameter. 3) (Optionally) the distance AB is less than a maximum "length" value specified by the third parameter.  This rule uses the has_spikes() built-in function which has the following parameters.  1) The geometry to test. 2) (optional) The maximum value for the sine of the angle in the spike (a real number in the range [0, 1]). Note: If omitted, this defaults to the sine of 1 degree (approximately 0.017). 3) (optional) The maximum length of the spike.  The action also uses the remove_spikes() built-in which removes all spikes using the same parameters as the find_spikes() built-in.  
This rule uses a 6 degree tolerance.
![Alt text](img/SpikeFix.png?raw=true "Spike Fixup Example")

### Action Syntax
For objects ALL: if (has_spikes(:ALL.geometry,sin(to_radians(6))) equals true) then let :ALL.geometry = remove_spikes(:ALL.geometry,sin(to_radians(6)))  
![Alt text](img/RemoveSpikesAction.png?raw=true "Spike Fix Action")

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
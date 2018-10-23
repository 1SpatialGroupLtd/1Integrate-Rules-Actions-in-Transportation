# Sample Road Checks
This repo folder contains some Sample Road checks by Rule that can be used within 1Integrate.  
These road checks are used by a number of State DOT's using 1Spatial's 1Integrate software.  
For instructions on restoring a backup please refer to the following [documentation](https://1spatial.com/documentation/1integrate/v2_3/Topics/Backup_Restore.htm?Highlight=Restore%20Backup%20Rules)

## [Roads Must Not Overlap Other Roads](RoadsDoNotOverlapOtherRoads.xml)
Overlaps occur when linear start/end points go past one another so a portion of Line A is on top of/underneath a portion of Line B.  This rule checks for those instances while also checking that roads do not self-overlap.
### Rule Syntax
Check for Road objects that there are no Road objects other for which Road:other.geometry overlaps Road.geometry and Road:other does not equal Road  
![Alt text](img/RoadsDoNotOverlap_Rule.png?raw=true "Overlaping Roads Rule Screenshot")

## [Road Network Must Not Contain Bifurcations](RoadNetworkMustNotContainBifurcations.xml)
Bifurcations are situations where a branch splits into 2 and the splits retain some of the branches attributes.  Check that there is at most only a single segment with the same STREET value intersecting the end points of road segments.
### Rule Syntax
Check for Road objects that there is at most 1 Road objects other for which Road:other.geometry intersects start_of(Road.geometry), Road:other.STREET equals Roads.STREET, and Road:other does not equal Road; AND there is at most 1 Road objects other for which Road:other.geometry intersects end_of(Road.geometry), Road:other.STREET equals Road.STREET, and Road:other does not equal Road.  
![Alt text](img/RoadNetworkMustNotContainBifurcations_Rule.PNG?raw=true "Bifurcations Rule Screenshot")

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
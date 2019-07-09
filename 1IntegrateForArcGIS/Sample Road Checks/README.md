# Sample Road Checks
This repo folder contains some Sample Road checks by Rule that can be used within 1Integrate.  
These road checks are used by a number of State DOT's using 1Spatial's 1Integrate software.  
For instructions on restoring a backup please refer to the following [documentation](https://1spatial.com/documentation/1integrate/v2_3/Topics/Backup_Restore.htm?Highlight=Restore%20Backup%20Rules) .

## [Road Geometry Must Not Be Too Short or NULL](SampleRoadChecks.rules)
Tests to see if a Road geometry is too short or NULL. For this example, a Road geometry needs to be longer than the data unit of length of 3. This rule uses the built-in function line_length. The line_length built-in function returns the length of a line. If the geometry passed in is a simple point or a simple area, then the length returned will be 0. If it is a complex geometry, the length returned will be the sum of the lengths of each simple line in the complex geometry. Note: This function does not currently fully support 3D geometries. Any 3D geometries will be projected down to 2 dimensions.

### Rule Syntax
Check for Road objects that Road.geometry does not equal null and line_length(Road.geometry) is greater than 3.    
![Alt text](img/RoadGeometryMustNotBeTooShortOrNull_Rule.png?raw=true "Short or NULL Rule Screenshot")

## [Road Network Must Be Void of Overshoots and Undershoots](SampleRoadChecks.rules)
Overshoots occur when Roads that should be intersecting at their start or end points go past where they are expected to intersect. Undershoots occur when Roads that should be intersecting at their start or end points come up short of where they are expected to intersect. In this example, a distance less than 5 in data unit length defines an overshoot or undershoot.
![Alt text](img/RoadOvershootUndershootExample.PNG?raw=true "Overshoot and Undershoot Example")

### Rule Syntax
Check for Road objects that (if there are no Road objects other for which Road:other.geometry intersects start_of(Road.geometry) and Road:other does not equal Road then there are no Road objects other for which Road:other.geometry is within a distance of 15 of start_of(Road.geometry) and Road:other does not equal Road) and (if there are no Road objects other for which Road:other.geometry intersects end_of(Road.geometry) and Road:other does not equal Road then there are no Road objects other for which Road:other.geometry is within a distance of 15 of end_of(Road.geometry) and Road:other does not equal Road)  
![Alt text](img/RoadNetworkMustBeVoidOfOvershootsAndUndershoots_Rule.png?raw=true "Overshoots and Undershoots Rule Screenshot")

## [Road Network Must Not Contain Bifurcations](SampleRoadChecks.rules)
Bifurcations are situations where a branch splits into 2 and the splits retain some of the branches attributes.  Check that there is at most only a single segment with the same STREET value intersecting the end points of road segments.
![Alt text](img/RoadBifurcationsExample.PNG?raw=true "Bifurcations Example")

### Rule Syntax
Check for Road objects that (there is at most 1 Road object other for which Road:other.geometry intersects start_of(Road.geometry) and Road:other.STREET equals Road.STREET and Road:other does not equal Road) and (there is at most 1 Road object other for which Road:other.geometry intersects end_of(Road.geometry) and Road:other.STREET equals Road.STREET and Road:other does not equal Road).
![Alt text](img/RoadNetworkMustNotContainBifurcations_Rule.png?raw=true "Bifurcations Rule Screenshot")

## [Roads Must Intersect at Start and End Points](SampleRoadChecks.rules)
Tests to see that Roads intersects other Roads at the start and end only, as opposed to anywhere along its geometry. This rule explicitly checks the four scenarios in which Roads may intersect each other: start to start, start to end, end to start, and end to end.

### Rule Syntax
Check for Roads objects that for all Road objects other for which Road:other.geometry intersects Road.geometry check that the start_of or end_of Road:other.geometry intersects the start_of or end_of Road.geometry.
![Alt text](img/RoadsMustIntersectAtStartAndEndPoints_Rule.png?raw=true "Roads Intersect at Ends Rule Screenshot")

## [Roads Must Not Contain Other Roads](SampleRoadChecks.rules)
Linear containment occurs when a Road geometry completely contains another Road geometry.  
![Alt text](img/RoadContainmentExample.PNG?raw=true "Linear Containment Example")

### Rule Syntax
Check for Road objects that for all Road objects other for which Road:other.geometry intersects Road.geometry check that start_of(Road:other.geometry) intersects start_of(Road.geometry) or start_of(Road:other.geometry) intersects end_of(Road.geometry) or end_of(Road:other.geometry) intersects start_of(Road.geometry) or end_of(Road:other.geometry) intersects end_of(Road.geometry)  
![Alt text](img/RoadsDoNotContainOtherRoads_Rule.png?raw=true "Road Containment Rule Screenshot")

## [Roads Must Not Overlap Other Roads](SampleRoadChecks.rules)
Overlaps occur when linear start/end points go past one another so a portion of Line A is on top of/underneath a portion of Line B.  This rule checks for those instances while also checking that roads do not self-overlap.  
![Alt text](img/RoadOverlapExample.PNG?raw=true "Overlap Example")

### Rule Syntax
Check for Road objects that there are no Road objects other for which Road:other.geometry overlaps Road.geometry and Road:other does not equal Road.
![Alt text](img/RoadsDoNotOverlap_Rule.png?raw=true "Overlapping Roads Rule Screenshot")

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
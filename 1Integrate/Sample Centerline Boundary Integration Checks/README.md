# Sample Centerline Boundary Integration Checks
This repo folder contains some sample rules to check the relationship between street centerlines and a boundary dataset (such as a township).  

In most street segmented data, the streets either split the boundary or the line segment starts and\or ends at the boundary.  Then there is an attribute in the street dataset that indicates which boundary is on the left and right side on the street.  

For instructions on restoring a backup please refer to the following [documentation](https://1spatial.com/documentation/1integrate/v2_3/Topics/Backup_Restore.htm?Highlight=Restore%20Backup%20Rules)


## [Streets Within Boundary](StreetsWithinBoundary.xml)
Checks that a street does not cross over a boundary. 
### Rule Syntax
Check for Street Segment objects that there are no Boundary objects for which Street Segment.geometry crosses Boundary.geometry
![Alt text](img/StreetsWithinBoundary_RULE.png?raw=true "Streets within boundary Screenshot")


## [Streets Within Correct Boundary](StreetsWithinCorrectBoundary.xml)
Checks that if a street is fully contained within a boundary (By fully contained, we are saying it does not share the boundary) then the street's left and right boundary names should both equal the boundary's name that the centerline is contained within.  

For this check, because a line is contained within a boundary even if it shares the edge, we need to make sure to add a clause to ignore street segments that are contained within the perimeter aka the built_in_function boundary().   

### Rule Syntax
Check for Street Segment objects that for all Boundary objects for which Street Segment.geometry is contained within Boundary.geometry check that Street Segment.LeftBoundaryName equals Boundary.NAME and Street Segment.RightBoundaryName equals Boundary.NAME and it is not the case that (boundary(Boundary.geometry) contains Street Segment.geometry)  
![Alt text](img/StreetsWithinCorrectBoundary_RULE.png?raw=true "Within Correct Rule Screenshot")


## [Check Right Left Boundary](CheckRightLeftBoundary.xml)
This checks that if the street segment shares the edge of a boundary, then the Streets Left Boundary value must equal the Boundary Name that is on the digitized left side of the street segment and conversely the Streets Right Boundary value must equal the Boundary Name that is on the digitized right side of the street segment.  

The check uses the built_in_function is_boundary_left() and is_boundary_right().  Which tests whether a line and an area have the boundary left\right relationship. That is, the area is on the left\right of the line, and the line is contained in the boundary of the area.  

This check will only work if the topology is correct.  A centerline must fully share the edge of a boundary.  
### Rule Syntax
Check for Street Segment objects that if there are exactly 2 Boundary objects for which Street Segment.geometry intersects Boundary.geometry then for all Boundary objects for which Street Segment.geometry intersects boundary(Boundary.geometry) check that (if is_boundary_left(Street Segment.geometry,Boundary.geometry) equals true then Street Segment.LeftBoundaryName equals Boundary.NAME) and (if is_boundary_right(Street Segment.geometry,Boundary.geometry) equals true then Street Segment.RightBoundaryName equals Boundary.NAME)
![Alt text](img/CheckLeftRight_RULE.png?raw=true "Check Left Right Rule Screenshot")


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
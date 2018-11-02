# Sample LRS Checks
This repo folder contains some Sample LRS or measured geometry checks by Rule that can be used within 1Integrate. 
LRS aka Linear Reference Systems are used by many transportation agencies for Asset Management.  Instead of storing a geometry associated with an asset, a non-spatial table is used with the Route ID, a Begin LRS, and an End LRS value is stored. The Begin and End LRS Value represents the distance along a route that the asset starts at and where the asset ends. Below is an example LRS Table.  
![Alt text](img/ExampleLRSTable.png?raw=true "Example LRS Table")   
These LRS checks are used by a number of State DOT's using 1Spatial's 1Integrate software.  
For instructions on restoring a backup please refer to the following [documentation](https://1spatial.com/documentation/1integrate/v2_3/Topics/Backup_Restore.htm?Highlight=Restore%20Backup%20Rules)


## [Begin LRS Exist On Route](BeginLRS_ExistOnRoute.xml)
Checks that the Begin LRS value in the LRS Table actually exists within the available measure values of the linear route.  This rule uses the built-in function get_measured_minimum and get_measured_maximum.  The get_measured_minimum returns the minimum measure value of a geometry. Measures can be null so the minimum may be null. The minimum value may not necessarily be found at the start/end of the geometry. If the argument passed in is null, the value returned will be null. If the argument passed in is not a measured geometry, an exception will be thrown.  The get_measured_maximum returns the maximum measure value of a geometry.  
### Rule Syntax
Check for Route objects that for all LRS_TABLE objects for which Route.ROUTE_ID equals LRS_TABLE.ROUTE_ID check that round(LRS_TABLE.Begin_LRS,1) is in the range [round(get_measure_minimum(Route.geometry),1),round(get_measure_maximum(Route.geometry),1)]  
![Alt text](img/BeginLRSonRoute_RULE.png?raw=true "Begin LRS Rule Screenshot")


## [End LRS Exist On Route](BeginLRS_ExistOnRoute.xml)
Checks that the End LRS value in the LRS Table actually exists within the available measure values of the linear route.  This rule uses the built-in function get_measured_minimum and get_measured_maximum.  The get_measured_minimum returns the minimum measure value of a geometry. Measures can be null so the minimum may be null. The minimum value may not necessarily be found at the start/end of the geometry. If the argument passed in is null, the value returned will be null. If the argument passed in is not a measured geometry, an exception will be thrown.  The get_measured_maximum returns the maximum measure value of a geometry.  
### Rule Syntax
Check for Route objects that for all LRS_TABLE objects for which Route.ROUTE_ID equals LRS_TABLE.ROUTE_ID check that round(LRS_TABLE.End_LRS,1) is in the range [round(get_measure_minimum(Route.geometry),1),round(get_measure_maximum(Route.geometry),1)]  
![Alt text](img/EndLRSonRoute_RULE.png?raw=true "End LRS Rule Screenshot")


## [Route Is Monotonic](RouteIsMonotonic.xml)
Checks to make sure a route is monotonic.  A Monotonic route is when the route does not contain either strictly increasing or strictly decreasing m-values on its vertices.  This rule uses the measured built-in function "is-measured_line_monotonic_increasing"  this function will return true if the input geometry is a measured simple line and if the measure values are increasing in the digitized direction along its length.  
### Rule Syntax
Check for Route objects that is_measured_line_monotonic_increasing(Route.geometry) equals true  
![Alt text](img/IsMonotonic_RULE.png?raw=true "Is Monotonic Rule Screenshot")


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
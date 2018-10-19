# Essential Geometry Checks
This repo folder contains the Essential Geometry Checks by Rule that can be used within 1Integrate.  
For instructions on restoring a backup please refer to the following [documentation](https://1spatial.com/documentation/1integrate/v2_3/Topics/Backup_Restore.htm?Highlight=Restore%20Backup%20Rules)

## Check for Duplicate Features
Checks for two features within the same class (aka Feature Class) that has the same geometry 
### Rule Syntax
Check for objects all that there are no objects duplicate features for which :duplicate features.geometry equals :all.geometry and class(:duplicate features) equals class(:all) and :duplicate features does not equal :all
![Alt text](img/DuplicateFeaturesRule.png?raw=true "Title")


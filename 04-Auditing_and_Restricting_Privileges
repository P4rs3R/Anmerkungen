##Discover local administrators
```
wmic /failfast:ON /node:@nodes.txt /output:local_admins.csv path win32_groupuser where (groupcomponent="win32_group.name=\"administrators\",domain=\"PC\"") get /format:csv
```
if you want to see ALL the users:
```
wmic /failfast:ON /node:@nodes.txt /output:users.csv path win32_groupuser get /format:csv
```

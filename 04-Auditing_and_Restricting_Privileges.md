
## Discover which services are running and allow/stop them
You can see services' status:
```
%windir%\system32\services.msc
```
(You can access the same window in many ways, for example from `compmgmt.msc`)

For example, if your hosts are using a static IP address, why don't you think about stopping "DHCP client"?
Stopping a service makes not only your hosts faster, but makes more difficult - for an adversary or an insider - executing a malicious attack such as privilege escalation.

:microscope: Take your time to find which service is absolutely needed and disallow all the rest.
---

## Discover local administrators
```
wmic /failfast:ON /node:@nodes.txt /output:local_admins.csv path win32_groupuser where (groupcomponent="win32_group.name=\"administrators\",domain=\"PC\"") get /format:csv
```
if you want to see ALL the users:
```
wmic /failfast:ON /node:@nodes.txt /output:users.csv path win32_groupuser get /format:csv
```

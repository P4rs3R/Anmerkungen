##Operating System: discover which one is installed and find missing Patches
```
wmic /node:@list.txt /output:OS_check.html os get BootDevice,SystemDrive,Caption,Version,CSName,CountryCode,InstallDate,LocalDateTime,OSArchitecture /format:htable
```
//where "list.txt" is the file where hosts (names or IP address) are written

//Improving: since the use of the network admin credentials is dangerous, you may consider to use "/user" and "/password", each time with the credentials of the local admin of the specified node
```
systeminfo /S PC /fo csv /nh >> OS.csv
```

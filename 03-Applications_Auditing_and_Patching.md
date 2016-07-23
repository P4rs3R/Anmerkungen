##Discover Applications and uninstall unwanted
```
wmic /node:@list.txt /output:apps_list.csv product get name,version,vendor /format:csv
```

When you find something you want to uninstall, just launch:
```
wmic /node:"[node name]" product where name="[application name]" call uninstall
```

or, when you want to uninstall a whole program suite:
```
wmic /node:"[node name]" product where vendor="[vendor name]" call uninstall
```

Obviously, as shown before, you can automate unistall replacing "node name" with a list of them

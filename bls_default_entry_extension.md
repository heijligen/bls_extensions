# Boot Loader Specification Default Entry Extension
Status: Draft, 2019-01-01, Thomas Heijligen <src@posteo.de>

The Boot Loader Specification has no feature for setting an entry that is loaded by default. 
Currently the entry choosing for automatic boot is done by the loader and the behavior may differ between individual implementations.
This Extension will add support unified behavior.

This extension adds a new file to $BOOT/loader/:
- default.conf

This extension specifies the entry that will loaded by default.
The file style equals the entry file style from original Boot Loader Spec.

If the $BOOT/loader/default.conf file is available and valid the loader should load the specified entry automatically.
Else the loader specific fallback should be used.

If $BOOT/loader/default.conf is not available at system installation the installer may add this file.
Else the installer should not modify it automatically. Only the system specified at $BOOT/loader/default.conf should 
update this file automatically. An user enforced update is always possible

The $BOOT/loader/default.conf file has following keys:
- *machine-id* refers to the machine-id key of the entry that should boot by default.
   This entry is required.

- *version* refers to the version key of the entry that should boot by default 
   or 'latest' for booting the highest version matching machine-id.
   This entry is required.
   
- *timeout* contains the time in seconds until the default entry will boot automatically. 
   This key is optional. When no key is given the default value should be 0 for boot immediately.


Example:
```
/boot/loader/default.conf

machine-id 6a9857a393724b7a981ebb5b8495b9ea
version    3.8.0-2.fc19.x86_64
timeout    5
```


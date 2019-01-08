# Boot Loader Specification Signed Entry Extension
## Status: Draft, 2019-01-01, Thomas Heijligen <src@posteo.de>


The Boot Loader Specification has no features for measured or verified boot.
This Extension will add support for it.

[Introduction about secureing the boot process]

[introduction and benefits of this  procedure]
On EFI systems the PE files have an embedded signature. Other filetypes haven't this feature. with saving the signature in seperatly 
we doesn't depent on a single file type. 
with storing the hash in the config file and signing this file we enshure that files we measure are those we want.
It give us the flexebility to have multible security level depending on for wich files we add checksums to the entry config file. 
it is possible to ship systems wich can only boot in a specific configuration when they are not unlocked.
On user controlled system the user signes his files himsilf to have full controll over the bootlabe system.


On efi systems the loader ($BOOT/EFI/..) stores the users public key and is signed wich efi stuff.
on open source firmware the loader in the firmware handles the public key management. 




This extention adds new files to $BOOT/loader/entries/:
- <machine-id><version>.sig

This extention adds new keys to $BOOT/loader/entries/  .conf files:
 - <key>+<hashf>

Each .conf file in $BOOT/loader/entries/ should be signed.
The signature should be stored in a file with the same name but a .sig suffix instead the .conf suffix.
The public key to verify the signature is stored in the firmware.

For each keys refers to an file there may be a new key holding checksum of this file.
The new key is made up of the original key and the type of the hash funktion seperated by a '+' e.g. linux+sha256.



If the signature veryfication fails the entry should not be bootable.
If one of the checksums fails the entry should not be bootable.
If the hash funktion is not known to the firmware the entry should not be bootable.
If a key storing a filepath has no signature key the system should be bootable.

```
/boot/loader/entries/6a9857a393724b7a981ebb5b8495b9ea-3.8.0-2.fc19.x86_64.conf

title         Fedora 19 (Rawhide)
version       3.8.0-2.fc19.x86_64
machine-id    6a9857a393724b7a981ebb5b8495b9ea
options       root=UUID=6d3376e4-fc93-4509-95ec-a21d68011da2
linux         /6a9857a393724b7a981ebb5b8495b9ea/3.8.0-2.fc19.x86_64/linux
linux+sha256  71761986337adac98229ad9c7752bb696a094e2bd317ea235ae4fe40cbef2184
initrd        /6a9857a393724b7a981ebb5b8495b9ea/3.8.0-2.fc19.x86_64/initrd
initrd+sha256 c84217e7f088c52f0a1df3fdcb0664df40ec93304214a2aea8f274a8d7f1464a
```

```
/boot/loader/entries/6a9857a393724b7a981ebb5b8495b9ea-3.8.0-2.fc19.x86_64.sig

[ Signature format not yet defined ]
```

## TODO
salt for hashing (/etc/shadow style)
measure file sizes to make hash collisions more deficult


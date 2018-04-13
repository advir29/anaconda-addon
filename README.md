# anaconda-addon
Amahi Anaconda Add-on

Directory Structure :-

```
root_folder
├─ ks
│  └─ __init__.py
├─ gui
│  ├─ __init__.py
│  └─ spokes
│     └─ __init__.py
├─ tui
|  ├─ __init__.py
|  └─ spokes
└─ __init__.py
```

Steps to make iso (everything in su) :-

First execute 'dnf -y install genisoimage isomd5sum'. Now begin :-


1. Download https://dl.fedoraproject.org/pub/fedora/linux/releases/25/Server/x86_64/iso/Fedora-Server-dvd-x86_64-25-1.3.iso into your working directory.

2. ```mkdir /mnt/iso```

3. ```mount -t iso9660 -o loop path/to/image.iso /mnt/iso```

4. ```mkdir ISO``` 

5. ```cp -pRf /mnt/iso ISO/```

6. ```umount /mnt/iso```

7. ```mkdir -p product/usr/share/anaconda/addons```

8. ```git clone https://github.com/amahi/anaconda-addon.git```

9. ```cp  anaconda-addon/isolinux.cfg ISO/iso/isolinux/ && cp anaconda-addon/grub.cfg ISO/iso/EFI/BOOT/```
   
   Note  - isolinux.cfg is for BIOS bootmenu and grub.cfg is for UEFI bootmenu. Both file contain lines beside every option:-
   
   a) inst.ks=hd:LABEL=Fedora-S-dvd-x86_64-25:/ks.cfg (to start the kickstart file saved in installation directory)
   
   b) inst.updates=hd:LABEL=Fedora-S-dvd-x86_64-25:/product.img (to load product.img saved in installation directory, where anaconda addon is saved. We will tell you that how you can create this file later.)

10. ```cp -f anaconda-addon/ks.cfg ISO/iso/```

11. ```cp -r anaconda-addon/org_amahi_setup product/usr/share/anaconda/addons/```

12. ```cd product/ && find . | cpio -c -o | gzip -9cv > ../product.img``` #this will create product.img file where addon is saved

13. ```mv -f ../product.img ../ISO/iso```

14. ```cd ../ISO/iso```

15. ```genisoimage -U -r -v -T -J -joliet-long -V "Fedora-S-dvd-x86_64-25" -volset "Fedora-S-dvd-x86_64-25" -A "Fedora-S-dvd-x86_64-25" -b isolinux/isolinux.bin -c isolinux/boot.cat -no-emul-boot -boot-load-size 4 -boot-info-table -eltorito-alt-boot -e images/efiboot.img -no-emul-boot -o ../Fedora_Server.iso . && implantisomd5 ../Fedora_Server.iso``` #your iso will be in path/to/ISO folder. Now boot it and enjoy.

    
    Note - in org_amahi_setup/ks/amahi_setup.py :-
    
    Check execute(self, storage, ksdata, instclass, users, payload): funtion 
    
    from where all commands get executed after installion.


Booting into ISO :-

1. Select your language.

2. On next screen, select your TimeZone, Keyboard.

3. On begin installation, Set your user and password with administrator.

4. Have a cup of coffee. :)

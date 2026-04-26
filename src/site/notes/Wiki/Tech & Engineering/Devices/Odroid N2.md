---
{"dg-publish":true,"permalink":"/wiki/tech-and-engineering/devices/odroid-n2/","title":"Odroid N2","dg-note-properties":{"type":"Page","collections":"Knowledge & Guides","title":"Odroid N2","aliases":null,"description":null,"icon":null,"createdAt":"2026-01-31T14:09:32.328Z","lastUpdated":"2026-02-11T13:10:32.628Z","tags":[],"coverImage":null}}
---

# Odroid N2

## Creating a bootable image

To create a bootable image on an SD card, download [Armbian Imager](https://imager.armbian.com/) and [run the following](https://github.com/armbian/imager/issues/67) to get the AppImage working on Wayland

```bash
./NAME.appimage --appimage-extract
rm squashfs-root/usr/lib/*wayland*so*
cd squashfs-root && ./AppRun
```

then select Vendor: Hardkernel -> Board: Odroid N2 -> Image: Whatever you like -> SD Card to write to. 

❗️ WARNING: You can also select your own drives here, do not overwrite your own files! Carefully check what device you want to write to before and after selection!

Put the Odroid boot switch selector in EMMC after having inserted the SD-Card. It should boot straight into linux without the firmware menu. 



## Device Details

The current IPv6 of the Odroid is: `2a02:8071:7112:b640:21e:6ff:fe42:17e0`, it is also reachable within the Tailnet as `odroidn2` or `100.113.92.74`. Please make sure your public key is in `~/.ssh/known_hosts`. 



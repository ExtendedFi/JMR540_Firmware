For this project system_unlocked.img is being modified

### Prerequisite
- [ubireader](https://github.com/onekey-sec/ubi_reader)
- mkfs.ubifs and ubinize (search, how to install it)


### Unpack

unpack system_unlocked.img
```bash
ubireader_extract_images system_unlocked.img -o system_u
cd ./system_u/system_unlocked.img/
```
Now you will get file with .ubifs 
In my case its "img-35657280_vol-rootfs.ubifs"

unpack ubifs
```bash
ubireader_extract_files -k -o ./rootfs img-35657280_vol-rootfs.ubifs
```

### Repack

repack .ubifs 
```bash
mkfs.ubifs -m 2048 -e 126976 -c 1073 -x lzo -f 8 -k r5 -p 1 -l 5 -F -r ./rootfs/ rootfs.ubivol
```

setting up config.ini
```bash
touch config.ini
echo "[rootfs-volume]
mode=ubi
image=rootfs.ubivol
vol_id=0
vol_size=27807744
vol_type=dynamic
vol_name=rootfs" > config.ini
```
if vol_size=27807744 try vol_size=26791936

Repack to img
```bash
ubinize -p 131072 -m 2048 -o system_m.img ./config.ini
```

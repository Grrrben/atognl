---
title: "Een SD kaart voor een camera formateren"
date: 2019-10-08T22:07:18+02:00
draft: false
type: post
---

Om een SD kaart voor een camera te formateren:

Eerst maar `lsblk` om de schijven en partities te zien. Een SD heet tegenwoordig `mmcblk0`.

```
NAME        MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
mmcblk0     179:0    0   7.4G  0 disk 
└─mmcblk0p1 179:1    0   7.4G  0 part 
nvme0n1     259:0    0 232.9G  0 disk 
├─nvme0n1p1 259:1    0 217.3G  0 part /
├─nvme0n1p2 259:2    0     1K  0 part 
└─nvme0n1p5 259:3    0  15.6G  0 part [SWAP]

```

Dan parted gebruiken. `fdisk` geeft _geen_ goed resultaat, de camera zegt dat de kaart niet goed geformateerd is. 
Let op de `mmcblk0`, dit kan anders heten: 

`sudo parted /dev/mmcblk0`

Dan binnen parted de volgende commando's:

```
(parted) mklabel msdos
(parted) mkpart primary fat32 1MiB 100%
(parted) set 1 boot on
(parted) quit
```

Formateren als fat32:

`sudo mkfs.vfat /dev/mmcblk0p1`

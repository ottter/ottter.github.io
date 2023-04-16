---
title: "TrueNAS and Plex"
summary: "Configuring TrueNAS and Plex"
date: 2022-03-17T15:21:55-04:00

tags: [homelab, proxmox, trueNAS, plex]
categories: [homelab, proxmox, trueNAS, plex]
draft: false
---

While I work on refining what I plan with my homelab and the purpose of these posts, I got the important part of this
project done, a Plex server.

### Configure HDD Passthrough

Prior to installing TrueNAS I had to do this so the drive I planned to store all the media on would show up properly.
**I don't have a proper PCI controller** on this setup yet so this is the next best thing.

Prior to TrueNAS install, in proxmox shell:

```bash
lsblk -o +MODEL,SERIAL
# find the serial to hdd
qm set 100 -scsi2 /dev/disk/by-id/ata-WDC_WD120EDAZ-xxxxxx
# 100 being the UID of the server on proxmox, so might vary
# model & serial in above command should match desired device from lsblk output
```

The installation was all pretty straight forward, but there was a couple items that either weren't covered in the instructions that I read, or are unique to my environment.

### Fix the Network

The network has to be configured so in TrueNAS, go over to Network > Global Configuration and enter:

* Nameserver: 8.8.8.8
* Default Gateway: 192.168.0.1

> TODO: Decide how I want the network configured now to save time later

### Permission configuration

Configuring the ACL to allow my Windows host to use SMB to drop files on to the NAS was a bit tricky

* Create a user (james) that you want to give rights to
* Create a group (plex-server) and add that user to the group
* Create the Samba share & configure the custom ACL

After installing the Plex plugin, this is how Plex knows where to find media, however it won't have access by default
![movies folders on plex](/images/homelab/plex-movies-1.png)

1. TrueNAS > Jails > Plex Shell > `$id plex`
2. TrueNAS > Storage > Pools > Edit Permissions > Add the ID as a new ACL item

> TODO: Create accounts for others to use with proper permissions

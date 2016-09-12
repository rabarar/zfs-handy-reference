# zfs-handy-reference
a handy cheat-sheet of commands for manipulating ZFS pools and filesystems


### Pools and Datasets

## zpool create 
zpool create poolname mirror /path/to/dev/sdb /path/to/dev/sdc 
zpool list

## export
zpool export poolname

## import
zpool import -d /path-to-files poolname

## zfs create 
zfs create poolname/docs 
zfs list -r poolname

## zfs snapshots
zfs snapshot poolname/docs@version1
zfs list -t snapshot

## destroy snapshots
zfs destroy poolname/docs@version1
zfs list -t snapshot

## rolling back snapshots
zfs snapshot poolname/docs@version1
zfs list -t snapshot
zfs rollback poolname/docs@version1

##  renaming snaps
zfs rename poolname/docs@version1 poolname/docs@version2
zfs list -t snapshot

## destroy dataset with snaps 
zfs destroy -r poolname/docs

### Clones - use a snapshot to create a r/w clone
zfs clone poolname/docs@today poolname/pict

## when done destroy the dataset then the snap
zfs destroy poolname/pict
zfs destroy poolname/docs@today

### Replication

## start with a snap and then use send and receive
zfs snapshot poolname/docs@today
zfs send poolname/docs@today | zfs receive poolname/backup

## Permissions to perform send and receive across machines using ssh
on the receiving machine:
         sudo zfs allow joeblow  compression,mountpoint,create,mount,receive poolname

on the sending machine:
        sudo zfs allow joeblow send,snapshot poolname/pic

## 
zfs send poolname/docs@today | ssh otherserver zfs recv poolname/backup


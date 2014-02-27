NOTE
====

davfs mount does not reflect sync settings in Box, everything will
be downloaded locally. This may potentially be a LOT of data.
Remember to exclude Box Sync directory from local backups.

DAVFS mount
===========

sudo chmod u+s /usr/sbin/mount.davfs
>> .davfs2/secrets
 https://dav.box.com/dav <email-address>@kth.se <your box secret>
chmod 0600 .davfs2/secrets

mkdir -p /tmp/box/mnt
>> /etc/fstab
 https://dav.box.com/dav /media/box.com davfs rw,users,noauto 0 0
mount https://dav.box.com/dav

Local synchronization
=====================

mkdir $HOME/Box\ Sync
sudo apt-get install unison


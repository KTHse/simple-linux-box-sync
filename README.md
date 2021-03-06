Naive Linux sync "client" for Box.com
=====================================

Unfortunately Box.com does not provide a native Linux sync client.
The supported fall back is to use davfs to mount the Box.com folder.
With the instructions in this file and the provided script it is
possible to implement a rudimentary sync client.

## Caveats

The davfs mount does not reflect sync settings in Box, so everything 
will be downloaded locally. This includes folders shared with you. 
This may potentially be a LOT of data. 

Consider the local sync folder a local cache. Do not store it on file
servers, remember to exclude Box Sync directory from local backups.

## Configurations

### DAVFS mount

I believe davfs is included by default in Ubuntu Linux, otherwise you'll
have to install it.

In order for a normal user to mount davfs you have to make it possible
to run the mount.davfs command as root. This script assumes this is done
by `sudo chmod u+s /usr/sbin/mount.davfs`

You have to add your davfs secrets to the file .davfs2/secrets. The secret
is created at https://kth.app.box.com/settings (for KTH users, it's in your 
settings at box.com)

Add a line to the file ~/.davfs2/secrets and limit the accessability of 
the file with `chmod 0600 .davfs2/secrets`

```https://dav.box.com/dav <email-address>@kth.se <your box secret>```

Add a line to the davfs configuration file ~/.davfs2/davfs2.conf (or /etc/davfs2/davfs2.conf)
to turn off locking.

```use_locks 0```

Add a line to /etc/fstab, possibly creating directories if you choose some
other path.

```https://dav.box.com/dav /media/box.com davfs rw,users,noauto 0 0```

### Unison

Install unison with `sudo apt-get install unison`

## Syncing

That should be it, execute the box-sync script and it should create a `Box Sync` folder
in your home folder and maintain a local copy of the Box.com files in it.

## Suggestions of things to work on

The script is very simplistic in several aspects. In particular it assumes that there is
only one davfs mount point (in fact, just one occurence of the string 'davfs') in the fstab.

* Replace the script with a more full featured client with user interface.
  * Handle configuration with client.
  * Taskbar integration.
  * System settings integration.
* Handle installation with dependencies by proper installation package.
* Use REST-api to read and respect sync settings for Box folders.
* Consider using unison graphic interface.
* Consider using incrond (inotify cron daemon).

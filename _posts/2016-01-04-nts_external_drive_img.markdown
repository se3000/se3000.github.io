---
layout: post
title:  "Note to self: Mountable .img Commands"
date:   2016-01-04 22:40:00
---
As I toy around with RPis, Tails, and reformatting computers I find myself looking these up these instructions over and over. To write an OS image onto an external drive:

1) Run `diskutil list`. Disks will be listed like `/dev/disk2`, find the disk you want to write to and note it's number, we'll call that number `X`.

2) Unmount the disk: `diskutil unmountDisk /dev/diskX`.

3) Run `sudo dd if=/Users/your/absolute-path.img of=/dev/rdiskX bs=1m`. (This part will probably take a couple minutes, see tip below.)

4) Once that is done, eject the disk: `diskutil eject /dev/diskX`.

And you're done!

### Pro Tip

If the waiting around on the third step gets you antsy and you want to keep track of the progress, I reccomend using the simple progress viewer tool. Run `brew install pv` with homebrew or whatever your equivalent pacakge manager command is.

Once `pv` is installed it's easier to operate as the super user, so `sudo su`.

Then, pipe Data Description command into and out of the progress viewer: `dd /Users/your/absolute-path.img | pv | dd of=/dev/rdiskX bs=1m`.

Now you've got an easy way to view your progress. When that command is done, don't forget to run `exit` to stop substituting the super user identity.

### Mostly Useless but Interesting

Writing to MicroSD cards for Raspberry Pis you likely have to use a MicroSD to SD converter. If so you might run into an error like `dd: /dev/disk4: Permission denied`. Turns out that flimsy, unlabelled, seemingly useless switch on the side of the converter is an ["intent indicator"](http://askubuntu.com/a/277160). Eject the SD card, flip that switch to allow writing, and start the whole process over from scratch. Turns out the disk permissions have nothing to with user permissions and everything to do with that flimsy little switch.

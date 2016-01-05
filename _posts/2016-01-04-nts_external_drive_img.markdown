---
layout: post
title:  "Note to self: Mountable .img Commands"
date:   2016-01-04 22:40:00
---
As I toy around with my Raspberry Pi, Tails, and reformatting computers I find myself looking these up these instructions over and over. So for the future, to write an OS image onto an external drive:

1. Find your disk number by running:  
`diskutil list`

2. Find the disk you want to write to. Disks will be listed like `/dev/disk2`. Note it's number, we'll refer to it as `diskX` instead of `disk2` in the commands below.

3. Unmount the disk:  
`diskutil unmountDisk /dev/diskX`

4. Copy the image over:  
`sudo dd if=/Users/your/absolute-path.img of=/dev/rdiskX bs=1m`  
This part is really where the magic happens and will probably take at least a couple of minutes. See below about viewing the progress.

5. Once that is done, eject the disk:  
`diskutil eject /dev/diskX`.  
And you're done!


### Pro Tip

If you're writing a large image or just don't like waiting indefinitely, you can keep track of the progress `pv` progress viewer command. Run `brew install pv` with homebrew or whatever your preferred pacakge manager command is.

Once `pv` is installed it's easier to operate as the super user. Run `sudo su` and enter the super user's password.

Next, pipe the Data Description command into and out of the progress viewer:
`dd if=/Users/your/absolute-path.img | pv | dd of=/dev/rdiskX bs=1m`.

Now you've got an easy way to view the command status. When the command is done, don't forget to run `exit` to stop substituting the super user identity.


### Mostly Useless but Interesting

When writing to MicroSD cards you'll likely have to use a MicroSD to SD converter. If so you might run into an error like `dd: /dev/disk2: Permission denied`. You'll probably notice a flimsy switch on the side of the converter, sometimes labelled "Lock," which never seems to actually prevent you from pulling out the Micro SD. Turns out that switch is an "[intent indicator](http://askubuntu.com/a/277160)," to mark whether you'd like the card to be writable or not. Apparently the disk permissions have nothing to with passwords/privileges and everything to do with that seemingly useless switch. While chuckling at the acronym "Secure Digital" eject the SD card, flip the switch to allow writing, and start the from the top.

# Create a custom ubuntu image

## Requirements

* squashfs-tools
    * `sudo apt-get update && sudo apt-get install squashfs-tools`
* deboostrap`
    * `sudo apt-get install debootstrap`
    
## 1 - download a filesystem 

You can download a filesystem using `debootstrap`, in our case, we are going to download ubuntu:

`debootstrap --arch $ARCH $RELEASE  $DIR $MIRROR`

* `$ARCH` is the architecture you are using (amd64, i386, ...)

* `$RELEASE` is the release you want to use

* `$DIR` is the directory where the filesystem will be downloaded

* `$MIRROR` is the place where deboostrap will retrieve the filesystem

The `--include` option allow you to add pre-installed package to your filesystem

This command is used to install Ubuntu:

`debootstrap --include ubuntu-minimal --arch amd64 bionic /tmp/ubuntu http://archive.ubuntu.com/ubuntu`

## 2 - mount the filesystem

Now you should see the directory with all your ubuntu files located in `/tmp/ubuntu`

Let's mount our filesystem:

`mount -t proc /proc /tmp/ubuntu/proc`

`mount --bind /dev /tmp/ubuntu/dev`

`mount --bind /dev/pts /tmp/ubuntu/dev/pts`

`mount --bind /sys /tmp/ubuntu/sys`

## 3 - enter inside ubuntu

Your filesystem is now mounted, so let's enter inside ubuntu:

`chroot /tmp/ubuntu /bin/bash`

Congratulation ! You are now inside ubuntu.

## 4 - Do what you want

Inside ubuntu, you can now do what you want. Install all your packages, add your startup scripts...

For example, you can add a script who will run a server on boot.

after that, type `exit` to leave ubuntu.

## 5 - unmount the filesystem

Lets unmount the filesystem:

`umount /tmp/ubuntu/proc`

`umount /tmp/ubuntu/dev`

`umount /tmp/ubuntu/dev/pts`

`umount /tmp/ubuntu/sys`

## 5 - Create a .squashfs file with your filesystem

Now we use mksquashfs to compress all our files:

`mksquashfs /tmp/ubuntu /tmp/ubuntu.squashfs`

You have now your custom root filesystem inside `ubuntu.squashfs`

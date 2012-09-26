# IEBox

An improved installer of the [IE Application Compatibility
VMs](https://www.microsoft.com/en-ca/download/details.aspx?id=11575) for Linux
and OS&nbsp;X using [VirtualBox](http://virtualbox.org/).

It is a fork of [ievms](http://xdissent.github.com/ievms/).

## Motivation

[Microsoft](https://www.microsoft.com) provides *free* [virtual
machines](https://www.microsoft.com/en-ca/download/details.aspx?id=11575) (VMs)
for testing websites in different Internet Explorer (IE) versions. Unfortunately setting
these virtual machines (VMs) up without
[Windows Virtual PC](https://www.microsoft.com/windows/virtual-pc/) is cumbersome. IEBox
aims to facilitate that process using [VirtualBox](http://virtualbox.org/) on Linux or
OS&nbsp;X. With a single command, you can have a running VM of either IE6, IE7, IE8 or
IE9.

## Requirements

*   [VirtualBox](http://virtualbox.org)
*   [curl](http://curl.haxx.se/)
    *   Ubuntu: `sudo apt-get install curl`
*   [unrar](http://home.gna.org/unrar/) (*Linux only!*)
    *    Ubuntu: `sudo apt-get install unrar`

## Installation

1.  Download `iebox.sh`

        curl -C - -O https://raw.github.com/ArloL/iebox/master/iebox.sh

2.  Make it executable

        chmod +x iebox.sh

## Usage

### Installing a VM

    ./iebox.sh -v 6

This will download all the necessary files to `~/.iebox` and install
the IE6 VM as *Windows XP IE6*.

To choose your own name use `-n "name"`. Example:

    ./iebox.sh -v 6 -n "Windows XP IE6 - Project"

These are the standard names also showing the operating system:

*   IE6: *Windows XP IE6*
*   IE7: *Windows Vista IE7*
*   IE8: *Windows 7 IE8*
*   IE9: *Windows 7 IE9*

### IE6 VM Network Drivers

When installing the IE6 VM, you **have** to install the network drivers upon
**first boot**. The drivers are already mounted as a CD in the VM.

If you don't install the drivers on first boot, an activation
loop will prevent subsequent logins forever. If this happens, restoring to
the `clean` snapshot will reset the activation lock.

### Clean Snapshot

A snapshot is automatically taken upon install, allowing you to easily revert
**all** changes made to the VM. Anything can go wrong in  Windows and rather
than having to worry about maintaining a stable VM, you can simply revert
to the `clean` snapshot.

### IE7, IE8, IE9 Windows Activation

In contrast to the Windows XP IE6 VM you can not activate the Windows Vista and
Windows 7 VMs. However there is a command that either resets the 30 day activation
period or - according to Microsoft - simply shuts down the VM and **resets** it
back to it's initial state:

    slmgr –rearm

I can not confirm the second behavior. In doubt revert the VM to the `clean` snapshot
(see [above](#clean-snapshot)).

### Resuming Downloads

If one of the comically large files fails to download, the `curl`
command used will automatically attempt to resume where it left off. 
Thanks, [rcmachado](https://github.com/rcmachado).

### Recovering from a failed installation

Each version is downloaded into a subdirectory of `~/.iebox/vhd`. For example
`~/.iebox/vhd/ie6`. If the installation fails for any reason (e.g. corrupted download),
delete the version-specific subdirectory and rerun the install.

If that does not help, try and see if VirtualBox already is listing the VM. If yes,
delete it and double check to delete all files from VirtualBox's default machine folder.

If nothing else, you can delete `~/.iebox` and rerun the install without
worrying about existing VMs (see [a note on directories](#a-note-on-directories)).


## Notes

### A Note on Disk Space

The images are massive and can take hours or tens of minutes to 
download, depending on the speed of your internet connection.
The Windows XP VM is by far the smallest with 400&nbsp;MB.
Since Windows XP supports IE6, IE7 and IE8, I recommend using the IE6
VM for testing IE7 and IE8 as well. Use the `-n` command-line parameter
for this:

    ./iebox.sh -v 6 -n "Windows XP IE8"

Also see [the best combination in terms of disk space](#the-best-combination-in-terms-of-disk-space).

### A Note on Directories

All the temporary downloads are placed in `~/.iebox`.
All the VMs are installed in the default folder of VirtualBox.
This means that the `~/.iebox` directory contains *no* necessary files.
    
## Combinations

These are combinations to set up VMs for all versions of IE.

### The Best Combination in Terms of Disk Space

This combination makes use of the fact that Windows XP supports IE6, IE7 and IE8.
IE7 and IE8 have to be installed manually, but the VM includes the necessary
installers.

    #!/bin/sh
    
    ./iebox.sh -v 6
    ./iebox.sh -v 6 -n "Windows XP IE7"
    ./iebox.sh -v 6 -n "Windows XP IE8"
    ./iebox.sh -v 9

### The Standard Machines

    #!/bin/sh
    
    ./iebox.sh -v 6
    ./iebox.sh -v 7
    ./iebox.sh -v 8
    ./iebox.sh -v 9

## Major differences to ievms

The goal was to make the shell script easier to use and more flexible.

*   You only install one VM at a time.
*   Use of command-line parameters rather than environment variables.
*   You can choose the name of the VM.
*   Only temporary files are saved in `~/.iebox` making it safe to be removed. Even
    when you have VMs installed (see next point).
*   Every VM's hard drive is copied to VirtualBox's default folder.

## Advanced options

### Specifying the install path

To specify where the required files are downloaded, use the `DOWNLOAD_PATH` variable:

    DOWNLOAD_PATH="/path/to/install/path" ./iebox.sh


### Passing additional options to curl

The `curl` command is passed any options present in the `CURL_OPTS`
environment variable. For example, you can set a download speed limit:

    CURL_OPTS="--limit-rate 50k" ./iebox.sh


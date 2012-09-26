# IEBox

Inspired by [ievms](https://github.com/xdissent/ievms).

## Major differences to ievms

*   You only install one VM at a time.
*   Use of command line parameters rather than environment variables.
*   You can choose the name of the VM.
*   Only temporary files are saved in `~/.iebox` making it safe to be removed.
    The VMs are installed in the default VBox machine folder.

## Usage

    ./iebox.sh -v 6

This will download all the necessary files to `~/.iebox` and install
the IE6 image as the VM *Windows XP IE6*.

You can also name the VM yourself:

    ./iebox.sh -v 6 -n "Windows XP IE7"

That gives you an easy way to use the **smaller** Windows XP IE6 VM for IE7.
It of course does not automatically install IE7 but the VM includes the
installer as well as a direct shortcut on the desktop.

To install the best combination in terms of disk space use the
`best-combination.sh` shell script:

    #!/bin/sh
    
    ./iebox.sh -v 6
    ./iebox.sh -v 6 -n "Windows XP IE7"
    ./iebox.sh -v 6 -n "Windows XP IE8"
    ./iebox.sh -v 9

To install all the standard machines use the `standard-machines.sh` shell script:

    #!/bin/sh
    
    ./iebox.sh -v 6
    ./iebox.sh -v 7
    ./iebox.sh -v 8
    ./iebox.sh -v 9

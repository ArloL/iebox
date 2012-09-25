# IEBox

Inspired by [ievms](https://github.com/xdissent/ievms).

## Major differences to ievms

*   You only install one image at a time.
*   You can choose the name of the VM.
*   Only temporary files in `~/.iebox`. It can safely be removed.
*   Command line parameters rather than environment variables.

## Usage

    ./iebox.sh -v 6

This will download everything to `~/.iebox` and install the IE6 image as
the VM *Windows XP IE6*.

You can also name the VM yourself:

    ./iebox.sh -v 6 -n "Windows XP IE7"

That gives you an easy way to use the **smaller** Windows XP IE6 image for IE7.

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

Building the Commander X16 ROMs
===============================

Building this source code requires [GNU Make] and the [cc65] assembler.
GNU Make is almost invariably available as a system package with any
Linux distribution; cc65 less often so.

- Red Hat: `sudo yum install make cc65`
- Debian: `sudo apt-get install make`

If cc65 is not available as a package on your system, you'll need to
install or build/install it per the instructions below.

Once the prerequisites are available, type `make` to build `rom.bin`.
To use that with the emulator, copy it to the same directory as
the `x16emu` binary or use `x16emu -rom .../path/to/rom.bin`.

> __WARNING:__ The emulator will work only with a contemporary version
> of `rom.bin`; earlier or later versions are likely to fail.


Building/Installing cc65
------------------------

### Linux Builds from Source

You'll need only basic build tools for this:
- Debian/Ubuntu: `sudo apt install build-essential git`

The cc65 source is [on GitHub][cc65]; clone and build it with:

    git clone https://github.com/cc65/cc65.git
    make -j4    # -j4 may be left off; it merely speeds the build

This will leave the binaries in the `bin/` subdirectory; you may
use thes directly by adding them to your path, or install them
to a standard directory:

    #   This assumes you have ~/.local/bin in your path.
    make install PREFIX=~/.local

### Building and Packages for Other Systems

Consult the Nesdev Wiki [Installing CC65][nd-cc65] page for some
hints, including Windows installs. However, the Debian packages they
suggest from [trikaliotis.net] appear to have signature errors.



<!-------------------------------------------------------------------->
[GNU Make]: https://www.gnu.org/software/make/
[cc65]: https://cc65.github.io/
[nd-cc65]: https://wiki.nesdev.com/w/index.php/Installing_CC65
[trikaliotis.net]: https://spiro.trikaliotis.net/debian

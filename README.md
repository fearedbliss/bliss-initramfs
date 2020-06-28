## Bliss Initramfs 8.0.0
#### Jonathan Vasquez (fearedbliss)
#### Designed for Gentoo Linux

## Description

An utility that generates an initramfs image with all files and dependencies
needed to boot your Gentoo Linux system installed on OpenZFS. This program was
designed as a simple alternative to genkernel for this use case.

## Usage

All you need to do is run the utility, select the options you want "a-la-carte",
and then tell the initramfs via your bootloader parameters in what order you
want those features to be trigered in. Check the USAGE file for examples.

## Kernel Parameters

The init script allows these parameters to be configured with boot time
kernel parameters:

### by

Passed to `zpool import` commands as the `-d` parameter.  Limits imports to
this directory or device.

e.g. `by=/dev/disk/by-uuid`

### enc_drives

Specifies the drives to be LUKS unlocked.  A comma separated list which
allows several shortcuts.  Each of `UUID=`, `PARTUUID=`, `LABEL=`, `PARTLABEL=`,
and `ID=` name the given (after the `=` sign) disk in the relevant
`/disk/by-...` path.  Any other string must start `/dev/` and name a particular
disk.

(If unset, will be prompted for at runtime.)

### enc_key

When `enc_type` is set to either `key` or `key_gpg`, set this to the path of
the file (within `enc_key_drive`) which holds the encryption key.

(If unset, will be prompted for at runtime.)

### enc_key_drive

The name of a disk which will be mounted, for reading the `enc_key` file.

(If unset, will be prompted for at runtime.)

### enc_key_ignore

Set to `1` to ... ?

### enc_options

This value is inserted verbatim into the `cryptsetup` command when performing
`luksOpen` actions.

### enc_targets

A comma separated list of names for the opened LUKS volumes.  Length should
match `enc_trives`.

If not specified the names will be `vault_0`, `vault_1`, and so on.

### enc_tries

The (maximum) number of tries to attempt when unlocking LUKS volumes.  E.g.
in case of a passphrase typo.  In case of this many failures, a rescue shell
will be spawned.

If not specified, the default is five.

### enc_type

The type of encryption key to use.  One of: `pass`, `key`, `key_gpg`.  See
other `enc_` options above.

### init

The name of the init script to run.  `/sbin/init` by default.

### options

Passed to `-o` of `mount` when mounting the root and (if applicable) `/usr`
paths.

### recover

If set to `1`, launches a rescue shell immediately after udev has started.

### redetect

When set to any non-empty value, will interactively prompt for re-detection
of (e.g. slow USB) drives during the boot process.

### refresh

When set to `1`, refresh the `zpool.cache`, by copying it into the root
file system after it has been mounted and also skip reading it from that
location.

### root

Required.  The path to the root filesystem.  For ZFS specify the pool and
dataset, e.g. `pool/root`.

### su

If set to `1`, switches to single user mode after mounting all drives but
before proceeding to a normal boot.

### triggers

A comma separated list.  Allowed values: `luks`, `zfs`.  Runs the named
subsystems, in order, during boot.

### usr

Optional, path of device containing the `/usr` partition, e.g. if it is separate
from the root partition.

## License

Released under the Apache License 2.0

## Dependencies

Please have the following installed:

- dev-lang/python 3.6+
- app-arch/cpio
- app-shells/bash
- sys-apps/kmod
- sys-apps/grep
- sys-fs/udev OR sys-fs/eudev OR sys-apps/systemd (UUIDs, Labels, etc)
- sys-apps/kbd (Keymap support)
- sys-fs/zfs (ZFS support)
- sys-fs/cryptsetup (LUKS support)
- app-crypt/gnupg (GPG Encrypted Keyfile used for LUKS)
- app-arch/gzip (initramfs compression)

For more information/instructions check the USAGE file.

## Contributions

Before submitting a patch, make sure to run `black` on the code.
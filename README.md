# ethname-pfsense
Forked version of the original [ethane](https://github.com/eborisch/ethname) rc.d script for re-naming system network interfaces based on their MAC addresses adapted for pfSense.

## Installation:

  1. Copy `ethname` into `/etc/rc.d/`
  2. Copy `README.md` to `/usr/share/ethname/`
  3. Copy `ethname8` to `/usr/local/man/man8/`
  
You can run the following commands from the directory where you extracted ethname:  

```
mkdir -p /etc/rc.d/
install -m 555 ethname /etc/rc.d/
mkdir -p /usr/local/ethname/
install -m 444 README.md /usr/local/ethname/
mkdir -p /usr/local/man/man8/
install -m 444 ethname.8 /usr/local/man/man8/
```

USAGE:

Add the following to rc.conf:
```
ethname_enable="YES"
ethname_<NAME>_mac="aa:bb:cc:dd:ee:ff"
```

For example:

```
ethname_enable="YES"
ethname_external_mac="aa:bb:cc:dd:ee:00"
ethname_private_mac="aa:bb:cc:dd:ee:01"
```

You can optionally restrict handling to a set of defined names with:
```
ethname_names="external private"
```
otherwise all defined `ethname_*_mac=""` values are used

Make sure any interfaces you want to rename have their drivers loaded or
compiled in. If externnal is on axe0, for example, add `if_load_axe="YES"` to
/boot/loader.conf. See the man page for your device (eg 'man axe') for
particulars.

All other devices are untouched.

Optional rc.conf settings:
```
ethname_timeout : Maximum wait time for devices to appear. [default=30]
```

That's it. Use `ifconfig_<name>=""` settings in rc.conf with the new names.

Supports device name swapping; uses temporary names as part of the process.

This version is has a new simplified interface, but will support the old
echname_map and ethname_devices configurations, as well.

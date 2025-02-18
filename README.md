# ethname-pfsense
Forked version of the original [ethname](https://github.com/eborisch/ethname) rc.d script for re-naming system network interfaces based on their MAC addresses adapted for pfSense.

## Installation

  1. Copy `ethname` into `/usr/local/etc/rc.d/`
  2. Copy `README.md` to `/usr/share/ethname/`
  3. Copy `ethname.8` to `/usr/local/man/man8/`
  
You can run the following commands from the directory where you downloaded the repository:  

```
mkdir -p /usr/local/ethname/

install -m 555 ethname /usr/local/etc/rc.d/
install -m 444 README.md /usr/local/ethname/
install -m 444 ethname.8 /usr/local/man/man8/
```

## Configuration

The recommended practice is to create a configuration file dedicated to ethname that is automatically parsed during boot.

### 1. Set up configuration file
Use nano or other preferred editor to create or open the file:

```
nano /etc/rc.conf.d/ethname
```
The file name must match the script file name we copied to `/usr/local/etc/rc.d/`, in our case `ethname`.

### 2. Enable ethnane
Enter the following line at the top of the config file:
```
ethname_enable="YES"
```
### 3. Assign NICs to MAC addresses:
Add the following line for each system network interface you like to assign:
```
ethname_<INTERFACE>_mac="<MACADDRESS>"
```
Replace \<NAME\> with your interface system name (eg `em0`, `vmx0`, `igb0` etc) and \<MACADDRESS\> with the value from your network adapter (eg `aa:bb:cc:dd:ee:ff`).

When done your file would look similar to the following example on a system with 4 intel NICs:

```
ethname_enable="YES"
ethname_em0_mac="aa:bb:cc:dd:ee:00"
ethname_em1_mac="aa:bb:cc:dd:ee:01"
ethname_em2_mac="aa:bb:cc:dd:ee:02"
ethname_em3_mac="aa:bb:cc:dd:ee:03"
```

Only the interfaces defined in the config file will be affected by the script, all other interfaces will remain untouched and will initialize in order determined by PCI bus sequence.

### 4. Save the updated config file

Exit nano by pressing `[ctrl] + x` followed by `y` and `[Enter]` to save changes.

### 5. Adjust config file ownership and access

Run the following command to make the file readable to rc:

```
chown root:wheel /etc/rc.conf.d/ethname
chmod 0644 /etc/rc.conf.d/ethname
```

This needs to be done only the first time when the file was created.
  
## Notes
  
For ethname to work make sure any interfaces you want to rename have their drivers loaded or
compiled in. If externnal is on axe0, for example, add `if_load_axe="YES"` to
/boot/loader.conf. See the man page for your device (eg 'man axe') for
particulars.

Ethname supports device name swapping without inflicts by utilizing temporary interface names when the script is executed.

## References

#### [Original ehtname repository](https://github.com/eborisch/ethname)
The vanilla FreeBSD version of ethname created and maintained by [Eric A. Borich](https://github.com/eborisch)

#### [Persistent NIC ordering/naming basedon MAC address(es)](https://forum.opnsense.org/index.php?topic=27023.msg145910)
Ethname implementation discussion with example use case for OPNSense similar to pfSense


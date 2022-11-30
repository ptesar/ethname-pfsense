# ethname-pfsense
Forked version of the original [ethane](https://github.com/eborisch/ethname) rc.d script for re-naming system network interfaces based on their MAC addresses adapted for pfSense.

## Installation

  1. Copy `ethname` into `/usr/local/etc/rc.d/`
  2. Copy `README.md` to `/usr/share/ethname/`
  3. Copy `ethname8` to `/usr/local/man/man8/`
  
You can run the following commands from the directory where you extracted ethname:  

```
mkdir -p /usr/local/ethname/

install -m 555 ethname /usr/local/etc/rc.d/
install -m 444 README.md /usr/local/ethname/
install -m 444 ethname.8 /usr/local/man/man8/
```

## Configuration

The recommended practice is to create a configuration file dedicated to ethname that is automatically parsed during boot.

   1. Create or open the file

      ```
      nano /usr/local/etc/rc.d/ethame.conf
      ```
   2. Enter the following line at the top if to the `rc.conf` file to enable ethnane
      ```
      ethname_enable="YES"
      ```
   3. Add the following line for each system network interface you like to assign to MAC address:
      ```
      ethname_<INTERFACE>_mac="<MACADDRESS>"
      ```
      Replace <NAME> with your interface system name (eg `em0`, `vmx0`, `igb0` etc) and <MACADDRESS> with the value from your network adapter (eg `aa:bb:cc:dd:ee:ff`).

When done your file would look similar to the following examples on a system with 4 intel NICs:
    
```
ethname_enable="YES"
ethname_em0_mac="aa:bb:cc:dd:ee:00"
ethname_em1_mac="aa:bb:cc:dd:ee:01"
ethname_em2_mac="aa:bb:cc:dd:ee:02"
ethname_em3_mac="aa:bb:cc:dd:ee:03"
```

Only the interfaces defined in the `rc.conf` will be affected by the script, all other interfaces will remain untouched and will initialize in order determined by PCI bus sequence.
  
## Notes
  
For ethname to work make sure any interfaces you want to rename have their drivers loaded or
compiled in. If externnal is on axe0, for example, add `if_load_axe="YES"` to
/boot/loader.conf. See the man page for your device (eg 'man axe') for
particulars.

Ethname supports device name swapping without inflicts by utilizing temporary interface names when the script is executed.

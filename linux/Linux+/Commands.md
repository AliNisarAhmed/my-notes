# Commands

| Parameter | Description                      |
| --------- | -------------------------------- |
| `$0`      | Name of command or script        |
| `$#`      | Number of command line arguments |
| `$*`      | List of command line arguments   |
| `$$`      | Current Process's PID            |
| `$!`      | PID of the last background job   |
| `$?`      | last Command exit code status                                 |


- `type -a <proposed_alias_name>`
	- to display any commands, aliases or functions currently using the proposed name
 

- `localectl list-locales`
- `localectl status`

- `# hwclock`
- `# hwclock -r`
- `# hwclock --show`


- `timedatectl list-timezones`
- `timedatectl set-timezone "<time zone>"`
	- changes the timezone
	- Behind the scenes, modifies the sym link b/w `/etc/localtime` and `/usr/share/zoneinfo/<Timezone>`
- `timedatectl set-time <HH:MM:SS | YYYY-MM-DD>`

- `ps -f`
	- lists processes with parent pids and user IDs




- `set`
- `set | grep <variable>`
	- find out if a shell variable has been defined
- `set -o`
	- view status of shell parameters
- `set -o <parameter>`
	- turns on the parameter, e.g. `allexport` which auto-exports all shell variables
- `set +o <parameter>`
	- turn off the parameter


- `env | grep <env_variable>`
	- find out if the env variable has been defined
 

- `<command> 2>&1`
	- redirects std error to the same location as std output 
	- e.g: `ls -ld /etc /doesNotExist > errorfile 2>&1` (redirect both to `errorfile`)
- `<command> 1>&2`
	- redirects std output to the same location as std error
	- e.g.: `ls -ld /etc /doesNotExist 2> errorfile 1>&2`
- `<command> >&`
	- redirects std out and stderr to the same file
	- e.g: `ls -ld /etc /doesNotExist &> errorfile`


- `chage -l <username>`
	- displays password aging information for a user
- `chage <username>`
	- open a text based interface to change user passwd aging info


- `passwd -S <username>`
	- displays password aging information for a user
- `passwd -e <username>`
	- immediately expire a user's password, forcing them to chance password on next login
 - `# passwd -l <username>`
	 - lock a username's password, prevents them from logging in
	 - requires root
 - `# passwd -u <username>`
	 - unlock the username's password as root


- `useradd -r <username>`
	- `-r` indicates that this is a system user
- `useradd -n`
	- negates the setting in `/etc/login.defs` and force the defaults in `/etc/default/useradd` to be used


- `userdel <username>`
	- removes user's record from `/etc/shadow` & `/etc/passwd`, but leaves the home directory
- `userdel -r <username>`
	- removes above + user directory + cron jobs + at jobs and mail


- `usermod -G <secondary_group_name> <username>`
	- add username to secondary group
	- NOTE: overwrites all previous secondary groups
	- use `usermod -aG` option to append
- `usermod -g <group_id> <username>`
	- make group_id for the user
- `usermod -U <username>`
	- unlock the username's account (similar to `passwd -l <username>`)
- `usermod -l <new_username> -d /home/<new_username> -m <original_user_name>`
	- change username for a user
	- home directory must be specified so that old directory contents can be copied to it


- `su <username>`
	- temporarily switch the user ID, primary group ID and home directory
- `su - <username>`
	- temporarily log on as another user and user their profile


- `echo file{a,b,c} | xargs touch`
	- create 3 files at one
	- xargs gets space delimited arguments and passes them each to `touch`
- `ls file* | xargs -I {} mv {} test.{}`
	- `-I` option sets the placeholder for xargs argument (here filea, fileb, filec)
- `find . -name "file[a-c]" | xargs -p \rm`
	- -p = print command before executing


- `typeset -f` (or `declare -f`)
	- lists all functions defined
- `unset <function_name>`
	- remove a function


- `hash`
	- show the hash table which stores the absolute paths to commands executed thus far with count
- `hash -r`
	- clear the hash table


- `type <command>`
	- displays how the keyword will be interpreted as a command



- `chattr <filename>`
	- change file attributes
	- ex: `chattr +i <file_name>`: make file immutable
	- ex: `chattr -i <file_name>`: remove immutable attribute
- `lsattr <file_name>`
	- list file attributes



- `getfacl <file_name>`
	- get ACLs defined on the file
- `setfacl -m u:<user_name>:r <file_name>`
	- set an ACL where user would only have read perms
- `setfacl -m mask:r <file_name>`
	- set mask ACL, i.e., define max permission possible on a file


- `ls -l /lib/modules/$(uname -r)/kernel/fs`
	- list available filesystem modules
- `cat /proc/filesystems`
	- determine which kernel filesystem modules are currently loaded
- `cat /etc/fstab`
	- automatically mount filesystems at startup we can add them to a file
- `df -T` or `df -h` or `df -hT`
	- see disk utilization and file system type
- `df -i`
	- show inode utilization
- `du -sh`
	- monitor disk usage in the directory where the command is run


- `mdadm -C /dev/md0 --level=1 --raid-disks=2 /dev/sdb /dev/sdc`
	- meta-device administration command uses `-C` to create a new meta device called `/dev/md0` at a RAID 1 level (disk mirroring), which will use two hard drives `/dev/sdb` and `/dev/dbc`)
- `mdadm --detail <meta_device_name>`
- `mdadm --fail`
	- mark the device as failed and prepare it for removal with `mdadm --remove`
	- Once the faulty hard drive has been replaced, reenable using `mdadm --add`
- `fstrim -av`
	- `-a`: trim all of the SSDs with TRIM features in RAID
	- `-v`: verbose


- `ldconfig -p`
	- view a list of all shared libraries available on your system
- `ldd -v <executable>`
	- view shared libraries required by the executable



- `dmesg`
	- read, display and control kernel ring buffer (a cyclical storage for logs)
	- `-f` -> facilities = `kern | user | mail | daemon | auth | syslog | lpr | news`
	- `-l` -> levels = `emerg | alert | crit | err | warn | notice | info | debug`
	- `-T` -> display timestamps
	- `-H` -> human readable
	- example: 
		- `dmesg -T -f <facility> -l <level>`



- `lsusb`
	- display a list of USB devices detected on the system
- `lsusb -t`
	- produces a hierarchical view of USB hubs and devices attached to hubs
- `lsusb -D /dev/bus/usb/<bus_number><device_number>`
	- display additional information on a device



- `lspci`
	- list PCI devices in format `Bus:Device:Function Class:Vendor Name`
	- `-tv` to display tree, and print more detailed info
- `lspci -nn | grep -i hba`
	- view HBA adaptors (host bus adapter is an expansion card used to connect multiple devices)
	- equivalent to command `systool -c <adapter_class>`
		- or looking for info in `/sys/class`



- `lsmod`
	- view all currently loaded kernel modules (pulls data from `/proc/modules`)
- `echo "blacklist <kernel_module> >> /etc/modprobe.d/<config_file>"`
	- blacklist a kernel module
- `modinfo <module_name>`
	- view more info abt a loaded module
- Load a kernel module
	- first run `depmod` to build file `/lib/modules/<kernel_version>/modules.dep`
		- lists dependencies between modules, helping kernel module utilities ensure that dependent modules are loaded whenever a module is loaded
	- Once the file is created, there are two options
		1. `insmod /lib/modules/<kernel_version>/kernel/<module_filename>`
			- e.g `insmod /lib/modules/$(uname) -r)/kernel/drivers/...`
		2. `modprobe <module_name>` (using the same directory for modules as above)
	- Most admins prefer latter command since `modprobe` installs module dependencies, while `insmod` does not
	- A module load is not persistent across system restarts, however, `modprobe` runs automatically every time the kernel loads
		- reads `/etc/modprobe.d/*.conf` files to determine which modules to load during startup
- Unload a currently loaded module
	- `rmmod -r <module_name>`
		- `-r` -> remove dependencies as well



- `lsblk`
	- view block devices
	- can also use `systool -pc block` (systool comes from `sysfsutil` package)



- `udevadm info <device_name_or_path>`
	- searches the `udev` database for device information
	- `-q, -n, -p` options for query, name and path
- `udevadm monitor`
	- listens for `uevents` and displays them on console
- `udevadm test --action <udev_rule>`
	- test a udev rule without implementing it
	- rules are added to file `/etc/udev/rules.d`



- `lsdev`
	- displays info about installed h/w, retrieving info from `/proc/interrupts`, `/proc/ioports` and `/proc/dma`
- `lshw`
	- used to display detailed h/w config info
	- replacement for `hwinfo`
	- `-short` - for displaying succinct info
	- `-class <storage | disk | volume>` 



- `systemctl status bluetooth`
	- check if bluetooth is active
- `bluetoothctl list`
	- display a list of available bluetooth controllers
- `bluetoothctl show`
	- displays a list of controllers and their status
- `bluetoothctl scan on`
	- displays a list of available controllers that have not been paired
- `hcitool scan`
	- scans & provides a controller's MAC address and name
- `bluetoothctl select <controller_MAC_address>`
	- apply any further `bluetoothctl` commands issued in the next 3 minutes
- `bluetoothctl`
	- Controller arguments
		- `agent on`
		- `discoverable on`
		- `discoverable off`
		- `pairable on`
	- Device Arguments
		- `info <device_MAC_address>`
		- `connect <device_MAC_address>`
		- `pair <device_MAC_address>`



- `ip link show`
- `ifconfig -a`
- `ncmli connection show`
- `ncmli device status`
- `netstat -i`
- `iw list`
- `iw <interface_name> info`
	- view status of interface, e.g. `iw wlp0s20f3 info`
- `iw <interface_name> essid <essid_name> [channel <channel>]`
	- connect to a wireless interface



- `hdparam`
	- `-v <device>`: displays disk settings
	- `-I <device>`: displays detailed disk statistics



- `lpadmin -p <printer_name> -v <device_name> -m raw`
	- add and configure printers and printer classes
		- a printer class is a group of printers associated with a single print queue
	- config info stored in `/etc/cups/printers.conf`
- `lpadmin -d`
	- sets the default system printer
 - `lpinfo -v`
	 - determine available devices (before creating a printer)
- `lpinfo -m`
	- determines what drivers are available
	- `lpinfo -m | grep <printer_name>`
- `lpstat -c`
	- display a list of printer classes
	- `lpstat -c -p <printer_name>`
		- check which classes a printer belongs to
- `lpstat -d`
	- determine the default printer
- `lpstat -o`
	- display all print jobs in all queues
- `lpq -P <queue_name>`
	- display jobs on a particular queue
 - `lpstat -p <printer_name>`
	 - display printer status
	 - Can also see visually on http://localhost:631
- `lp -d <printer_name> <file_path>`
	- print a file
	- returns a job id
	- cancel with `cancel <job_id>` or `cancel <queue_name>`
- `lpmove <old_print_queue> <new_print_queue>`
	- move all jobs
- `lpmove <job_id> <printer_queue>`
	- move a job to a particular queue
- `cupsreject <queue_name>`
- `cupsaccept <queue_name>`
- `cupsdisable <printer_name>`
- `cupsenable <printer_name>`



- `ip addr add <ip_addr> dev <interface>`
- `ip addr del <ip_addr> dev <interface>`
- `ip link set <interface> down`
	- disable an interface
- `ip link set <interface> up`
	- enable an interface
	- also:
	- `systemctl restart network`
	- `systemctl restart networking`



- `hostnamectl set-hostname server-one`


- `dhclient <interface>`
	- acquire ip addr for this interface from DHCP server


- `route add -net <netw_addr> netmask <netmask> gw <router_addr>`
	- add routes to the host's routing table
	- e.g. `route add -net 192.168.2.0/24 gw 10.0.0.254`
- `route del -net <network_addr> netmask <netmask> gw <router_addr>`
- `route add default gw <router_addr>`
	- set the default route
- `route` or `ip route show`
	- show routing table
- `ip route add <network/prefix> via <router_ip_addr> dev <interface>`
	- add a static route to the routing table
- `ip route del <network/prefix>`



- `iptables -t <table> <command> <chain> <options>`
	- commands
		- `-L`: List all rules in the chain
		- `-N <chain_name>`: Creates a new chain
		- `-I`: Insert a rule into the chain
		- `-R`: Replaces a rule in the chain
		- `-D <chain_name> <rule_number>`: Deletes a rule from the chain
		- `-F`: Deletes all rules from the chain (called *flushing*)
		- `-P <chain_name> <policies>`: sets the default policy for the chain
			- policies `ACCEPT | DROP | QUEUE | REJECT`
		- `-A INPUT`: configure rule for all packets
	- Options
		- `-p`: specifies protocol to be checked by the rule
			- `all | tcp | udp | icmp`
		- `-s <ip_addess>/<mask>`: Specifies the source address to be checked. 
			- to check all IP addrs: use `0/0`
		- `-d <ip_addr>/<mask>`: Specifies the destination address to be checked
			- to check all IP addrs: use `0/0`
		- `-j <target>`: Specifies what to do if the packet matches the rule
			- values: `ACCEPT | REJECT | DROP | LOG`
		- `-i <interface>`: Specifies the interface where a packet is received
			- applies only to `INPUT` and `FORWARD` chains
		- `-o <interface>`: Specifies the interface where a packet is to be sent
			- applies only to `OUTPUT` and `FORWARD` chains
	- Default Chains
		- `FORWARD`: Contains rules for packets being transferred b/w networks thru the Linux systems
		- `INPUT`: Contains rules for packets that are being sent to the local Linux system
		- `OUTPUT`: Contains rules for packets that are being sent from the local Linux system




- `gpg --list-keys`
- `gpg --gen-key`
	- Generate gpg key
	- use RSA and 2048
	- Provide real name, email, comment and passphrase
- `gpg --export-secret-keys --armor <key_owner_email> > <filename>.asc`
	- save this on a USB drive as a backup
- `gpg -e -r <key_user_name> <filename>`
	- encrypt a file using gpg
- `gpg --output <output_filename> --decrypt <encrypted_filename>`
	- decrypt a file encrypted with gpg
- `gpg --keyserver <server_addr | hkp://subkeys.pgp.net> --send-key <key_id>`
	- copy public key to a key server
	- to obtain key id:
		- `gpg --fingerprint <key_owner_email>`
- `gpg --export --armor <key_owner_email> > <public_file_name>`
	- export the public key to a file, and then use `scp` to send a file to user
- `gpg --import <pub_key_file>`
	- import a gpg key
- `gpg --output revoke.asc --get-revoke <KEY_ID>`
	- create key revocation certificate
	- store it in a USB drive



- `rm /lib/modules/$(uname -r)/kernel/drivers/usb/storage/usb-storage.ko.xz`
	- Disable USB ports



- `chage <option> <user>`
	- configure password aging
	- `-m <days>` - min no. of days b/w password changes
	- `-M <days>` - max number of days b/w passwd changes
	- `-W <days>` - number of warnings days b4 a passwd change 
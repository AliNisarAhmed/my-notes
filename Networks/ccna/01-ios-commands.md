
### Modes & Interfaces
1. `conf t`
	- Go to Global Config
2. `enable`
	- go from USER EXEC to PRIVILEGED EXEC
3. `interface <interface_name>`
	- Go to Interface config mode
4. `show ip interface brief`:
	- show interface, IP addr, and ports
	- shortcut: `sh ip int br`
5. `show interface <interface_number>`: 
	- e.g. `show interface gig0/0`
	- show details about an interface
	 - can be used to find speed and duplex mode
6. `show interfaces` or `show interfaces <interface_id>`
	- shows various counters of errors detected on an interface
7. `show interfaces description`
	- shows the description column (in addition to what `show interface` command shows) 
	- helpful in identifying the purpose of each interface 
	- set description using `description <description>` (requires interface config mode)
 8. `show interfaces status`
	 - **NOTE**: command only works on Switches
	 - shows name (description), status, vlan, duplex, speed and type of all interfaces
 9. `hostname <host_name>`
	  - sets hostname (requires interface config mode)
 10. `duplex full` vs `duplex half` vs `duplex auto`
 11. `speed 100`
	 - to set speed to 100Mbps
 12. `show run` vs `show run interface <interface_id>`
	 - show running config vs show running config for a particular interface
 13. `show version`
	 - show IOS version
 14. `interface range f0/5 - 12`
	 - Enter **interface range config** mode, configuring multiple instances at once
	 - in the above exampe, we are confuguring interfaces 5-12
	 - can also do non-continuous ranges like `int range f0/5 - 6, f0/9 - 12`

### IP addr
 1. `ip adrress <ip_address> <subnet_mask>`
	 - set IP address on an interface (requires interface config mode)
	 - subnet mask should be in dotted decimal (NOT slash notation)
 2. `shutdown` and `no shutdown`
	  - disable and enable an interface (requires interface config mode)
 3. `ip default-gateway <default-gw-ip>` 
	  - requires global config mode

### MAC Addresses & Routing
1. `show mac address-table dynamic`:
	- show dynamic MAC addresses on a switch
2. `clear mac address-table dynamic`
3. `show ip route`:
	- view Routing table

### ARP
1. `show arp` (View arp)
2. `clear arp-cache` 


### DNS Commands
- DNS Client
	- `ip domain lookup` (enable DNS on a router / enable router to resolve hostnames)
	- `ip name-server <172.23.4.1>` 
		- (configure DNS server on the IP)
		- IP can be same as the current Router, in which case it becomes a server
		- else, it can point to a DNS server, hence the current router becomes a client
	 - `ip domain-name flackboxA.lab` (primary domain name)
	 - `ip domain-list flackboxB.lab` (additional DNS suffixes to search)
  - Additional DNS Server commands:
	  - `ip dns server` (once set up is done, start the DNS server)
	  - `ip host LinuxA 172.23.4.2` (Add host (either name or FQDN) against DNS for the IP)


### CDP
- `cdp run` 
	- enabled by default though
- `no cdp run`
	- disable cdp globally (requires global config mode)
- `no cdp enable`
	- requires interface config mode
	- disable cdp on an interface
	 - UseCase: for hiding info about this interface to other devices usually external
		 - do not forget to run `no cdp run` and then `cdp run` on the external device to flush their cache
- `show cdp`
	- shows whether enabled or not
- `show cdp neighbors`
	- shows a summary of neighbors
- `show cdp neighbors detail`


### LLDP
- `lldp run`
- `no lldp run`
- `no lldp transmit`
- `no lldp receive`
	- disable transmit and receive at interface level
 - `show lldp`
 - `show lldp neighbors`
 - `show lldp neighbors detail`


### Flash & Memory
- `boot system flash:<file_name_in_flash>`
	- choose the IOS image file which boots on system start
- `show flash`
	- list system images on Flash
- `delete flash:<file_name>`
	- delete flash `file_name`
 - `write erase`
	 - delete the startup config
	 - run `reload` to reload the device to boot up with a blank configuration
 - `config-register`
	 - can be used to change the way the router boots
	 - use in global config mode
	 - in rommon mode, this command changes to `confreg XXXX`
	 - e.g. `config-register 0x2142`
	 - options
		 - `0x2102` - boot normally
		 - `0x2120` - boot into rommon
		 - `0x2142` - ignore contents of NVRAM (startup config)
 - `copy flash tftp` 
	 - copy system image to a TFTP server
	 - vs `copy running-config tftp` 
	 - vs `copy startup-config usb`
	 - vs `copy flash start` (and so on)
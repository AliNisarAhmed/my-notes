
CheatSheet: https://www.netwrix.com/cisco_commands_cheat_sheet.html


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
 15. `default interface <interface_id>`
	 - reset an interface to its default config

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
4. `mac-address <mac_address>`
	- configure MAC address for an interface (requires interface config mode)

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

### Routes
- `show ip route`
	- show routing table
	- starting IOS v 15 also shows router IP
- `ip route <target_network> <subnet_mask> <next_hop_address>`
	- add a static route to `target_network` using `next_hop_address`
	- `next_hop_address` is IP address of the directly connected router
- `ip route <target_network> <subnet_mask> <exit_interface>`
	- instead of next hop IP addr, we can also specify a Router's own interface name from where the Router will exit/output the traffic
	- e.g: `ip route 192.168.1.0 255.255.255.0 g0/0`
	- uses `Proxy ARP` protocol to find next hop, and shows up in routing table as a "Connected" route
- `ip route <target_network> <subnet_mask> <exit_interface> <next_hop>`
	- combine the 2 commands above
	- does not use proxy ARP, and appears as a Static route not directly connected
- `ip route <target_n/w> <subnet_mask> <next_hop_address> <Administrative-Distance: Int>
	- makes this route a Floating Static Route
	- e.g. `ip route 10.0.1.10 255.255.255.0 10.1.3.2 115`


### Pipe operator
- `show running-config | include ip route`
- `show run | section rip`
- `show run | begin rip`



### Debug
- `debug`
	- debug is like show, but prints updates as they happen
	- e.g. `debug ip rip` - shows info as RIP exchanges happen
- `undebug all`
	- stops all debugging info



### Protocols
- `show ip protocols`
- `show ip rip database`
	- shows all the routes learned from the neighbors
- `show ip ospf database`
- `network <ip_add>`
	- in router config mode, associates a network with a routing protocol



### Loopback interfaces
- `interface loopback <interface_number>`
	- ceates a loopback interface with a virtual interface of <interface_number>
	- used to configure a single IP address for a router, so that it can be reached via that IP no matter the path available
	- best practice to give loopback addr an IP of /32 (any IP can be used)
		- e.g. `ip address 192.168.1.1 255.255.255.255`
- `passive-interface <loopback_interface`
	- sets the loopback interface as a passive interface


### VLAN
- `show vlan brief`
	- displays existing vlans and interfaces on them
	- only shows the access ports assigned to each VLAN
		- NOT the trunk ports that allow each VLAN
 - Assigning interfaces to vlans
	 - `interface range g1/0-3`
	 - `switchport mode access`
	 - `switchport access vlan 10`
 - `vlan <vlan_number>`
	 - enter config mode for vlan
 - `name <vlan_name>`
	 - rename a vlan from vlan config mode
 - `no vlan <vlan_number>`
	 - delete a vlan
 - Trunk Configuration
	 - `switchport trunk encapsulation dot1q` 
		 - (not needed on switches which only support dot1q)
		 - default: `switchport trunk encapsulation negotiate`
	 - `switchport mode trunk`
 - `show interfaces trunk`
	 - see all trunk switchports
 - Allowing vlans on trunk
	 - `switchport trunk allowed vlan <comma_separated_vlan_ids>`
		 - configure these vlan ids to be allowed on trunk port
	 - `switchport trunk allowed vlan add <vlan_id>`
		 - add this vlan to allowed list
	 - `switchport trunk allowed vlan remove <vlan_id>`
		 - remove this vlan from the allowed list
	 - `switchport trunk allowed vlan all`
		 - allow all vlans on this trunk port
		 - default settings, by default all vlans are allowed
	 - `switchport trunk allowed vlan except <comma_sep_vlan_ids>`
		 - allow all vlans except these vlans
	 - `switchport trunk allowed vlan none`
		 - No vlans allowed
 - `switchport trunk native vlan <vlan_id>`
	 - change native vlan
 
 
### Router on a Stick (ROAS)
- `interface g0/0.<sub_interface_number>`
	- enter interface config mode for sub-interface
	- recommended to match `sub_interface_number` with `<vlan_number>` below
- `encapsulation dot1q <vlan_number>`
	- instructs router to behave as if all frames arriving for `vlan_number` arrived for the sub-interface we are configuring
	- router tags the frame with vlan number and sends it to appropriate vlan
- `ip address <ip_address>
	- assign IP to the sub-interface
- Configuring Native vlan on a router
	1. `encapsulation dot1q <vlan_id> native`
		- on the router sub-interface
	2. configure IP addr for native VLAN on router's physical interface


### Multi-Layer Switch (Layer 3 Switch)
- `SW(config)# ip routing`
	- enables Layer 3 routing on the switch
	- requires global config mode
- `SW(config-if)# no switchport`
	- This configures the interface as a *routed port* (Layer 3 port, not a Layer 2/switchport)
	- requires interface config mode
	- vs `ip routing`
		- `ip routing` enables Routing on a switch
		- `no switchport` configures an SVI as a routed port
- `SW(config)# interface vlan <vlan_number>`
	- create an SVI for a VLAN
	- assign IP and use `no shutdown` to enable SVI


### DTP
- `show interfaces <interface_id> switchport`
	- show Adminstrative mode and Operational mode
- `switchport mode dynamic desirable`
	- switchport will actively try to form a trunk with other switches in the following modes
		- `switchport mode trunk`
		- `switchport mode dynamic desirable`
		- `switchport mode dynamic auto`
- `switchport mode dynamic auto`
	- default administrative mode
	- switchport does not actively try to form trunk with other switches
	- unless the other switch is actively trying to form a trunk, in the following modes
		- `switchport mode trunk`
		- `switchport mode dynamic desirable`
- `switchport nonegotiate`
	- disable DTP negotiation on an interface
	- recommended to disable
	- `switchport mode access` also disables DTP neg.


### VTP
- `show vtp status`
- `vtp domain <domain_name>`
- `vtp mode server`
	- default
	- van modify/add/delete VLANs
- `vtp mode client`
	- cannot modify/add/delete VLANs
- `vtp mode transparent`
	- a switch in this mode
		- will forward vtp advertisements
		- will not sync its db to the VTP server
		- will not advertise its own db
		- can add/modify/delete VLANs
- `vtp version <version_number>`
	- version number is 1 or 2 or 3
	- version 1 is the default


### STP
- `show spanning-tree`
	- shows SP Tree for all VLANS
- `show spanning-tree vlan<number>`
	- show SP Tree for the VLAN
- `show spanning-tree detail`
	- like above, but with more detail
	- shows root cost of an interface
- `show spanning-tree summary`
- `R1(config-if)# spanning-tree portfast`
	- enable portfast (or Edge link type on RSTP)
	- should only be done on ports connected to single end hosts
	- requires interface config mode
- `R1(config)# spanning-tree portfast default`
	- enables portfast on all access ports (not trunk ports)
	- requires global config mode
- `spanning-tree bpduguard enable`
	- requires interface config mode
- `spanning-tree portfast bpduguard default`
	- enables BPDU Guard on all Portfast-enabled interfaces
	- requires global config mode
- Spanning tree config
	- `spanning-tree mode ?`
		- `mst`
		- `pvst`
		- `rapid-pvst`
	- `spanning-tree vlan <vlan_number> root primary`
		- sets the STP priority to 24576
		- if another sw has Pr < 24576, it sets this sw Pr to 4096 less than other's
	- `spanning-tree vlan <vlan_number> root secondary`
		- sets the STP priority to 28672
	- `spanning-tree vlan <vlan_number> cost <cost>`
		- change SP cost anywhere from 1 - 200M
	- `spanning-tree vlan <vlan_number> port-priority <pr>`
		- pr = 0-224 in increments of 32
- `spanning-tree link-type point-to-point`
	- configures an interface as point-to-point link type (ie. b/w 2 sw)
	- auto enabled, but can be manually enabled with the above command
- `spanning-tree link-type shared`


### EtherChannel

[[05-EtherChannel]]

- `show etherchannel load-balance`
	- shows how etherchannel is load balancing
- `port-channel load-balance <load_balance_method>`
	- requires  global config
	- choices
		- `dst-ip`
		- `dst-mac`
		- `src-dst-ip`
		- `src-dst-mac`
		- `src-ip`
		- `src-mac`
- `channel-group <virtual_interface_number> mode <mode>`
	- configure an interface to join an Ethernet Channel
	- requires interface config mode (commonly done with `interface range`)
	- To configure PAgP
		- mode
			- `auto` (like DTP auto)
			- `desirable` (like DTP desirable)
	- To configure LACP
		- mode
			- `active` (like desirable above)
			- `passive` (like auto above)
	- To configure static EtherChannel
		- `on` only works with `on` mode (will not work with desirable or active)
- `channel-protocol <etherchannel_protocol>`
	- manually sets protocol (usually not needed as auto configured)
	- protocols
		- `lacp`
		- `pagp`
- `interface port-channel <etherchannel_identifier>`
	- configure and create/establish etherchannel on an interface
	- requires interface config mode for port channel created before
- `show etherchannel summary`
- `show etherchannel port-channel`
- Layer 3 EtherChannel
	- `int range g0/0-3`
	- `no switchport` (make interfaces Layer3 Routed interfaces)
	- `channel-group 1 mode <active | on | desirable>`
	- Now configure IP address on port-channel
	- `int po1`
	- `ip address 10.0.0.1 255.255.255.252`


### Floating Static Routes

- `ip route <network_addr> <subnet_mask> <next_hop> <AD>`
	- where AD is bw 1 to 255


### RIP
- `router rip`
	- enters RIP config mode
	- represented by `R1(config-router)#`
	- most of the commands below require R  config mode
- `version 2`
	- use RIPv2
- `no auto-summary`
	- use CIDR
- `network <addr>`
	- note: network command is classful 
	- even if you enter CIDR addr, it will be converted to respective classful addr
	- no need to enter the subnet mask
	- e.g. `10.0.0.1` is treated as `10.0.0.0` (Class A)
	- `10.0.0.0` is assumed to be `10.0.0.0/8`
- `passive-interface <interface_id>`
	- sets an interface as passive
	- NOTE: requires R config mode
- `default-information originate`
	- share default route info to RIP neighbors
- `show ip protocols`
	- shows the current IP protocol enabled
- `maximum-paths <1-32>`
	- set maximum paths
- `distance <1-255>`
	- change the AD


### EIGRP
- `router eigrp <AS number>`
	- AS number must match bw all R on same EIGRP
- `no auto-summary`
- `passive-interface <interface_id>`
- `network <nw>`
- `network <nw> <wildcard_mask>`
	- NOTE: uses wildcard mask instead of subnet mask
- `eigrp router-ip <id>`
	- id is not a network, just a unique ID in address format
- `show ip eigrp neighbors`
	- show EIGRP neighbors
- `show ip route <filter>`
	- filter routes
	- filter = `eigrp | ospf | connected | static | local`
- `show ip eigrp topology`
- `eigrp router-id a.b.c.d`


### Loopback Addr
- `interface loopback <integer>`
	- create a loopback interface on a R
	- then use `ip addr <addr>` to configure an IP addr (usually /32)


### OSPF
- `router ospf <process_id::1-65535>`
- `network <nw_addr> <wildcard_mask> area <area_number>`
	- usually area number = 0
	- 4 ways to enable OSPF on an interface
		1. `network 0.0.0.0 255.255.255.255 area 0`
			- enables OSPF on all interfaces at once
			- not recommended in real interfaces
		2. `network 10.0.13.2 0.0.0.0 area 0`
			- this one uses the exact interface ip address to match, thus with /32 wildcard mask
		3. `network 10.0.0.0 0.0.255.255 area 0`
			- this matches networks `10.0.34.0/30` and `10.0.13.0/30`
		4. `network 10.0.12.0 0.0.0.3 area 0`
			- this matches the exact network addr, thus the /3 wildcard mask
- `passive-interface <interface_id>`
	- requires OSPF config mode
	- stops OSPF hello msg propagation through that interface
- `default-information originate`
- `maximum-paths <1-32>`
	- maximum number of paths an OSPF router will use to perform ECMP load-balancing
	- default 4
- `distance <1-255>`
	- Modify the OSPF AD on the local router
- `clear ip ospf process`
	- Reset the OSPF process on the local Router
	- requires global config mode on a R
- `show ip ospf database`
	- show LSDB
	- required Global config more
- `show ip ospf neighbor`
	- show ospf neighbors and OSPF state
	- requires global config mode
- `show ip ospf interface`
	- show details for all interfaces
- `show ip ospf interface <interface_id>`
	- shows OSPF details for this interface, like Cost, Hello and Dead timers
- `auto-cost reference-bandwidth <b/w>`
	- requires OSPF config mode
	- change the reference cost to b/w which should be in Mbps
- `ip ospf cost <cost::1-65535>`
	- manually configure cost of an interface
	- requires interface config mode
	- this cost takes priority over the auto-configured cost
- `bandwidth <b/w>`
	- manually change the b/w of an interface
	- b/w is in kbps
	- this also affects OSPF cost calculation
	- requires interface config mode
	- NOT RECOMMENDED to change the b/w manually 
		- as it is used in many places besides OSPF
- `R1(config-if)# ip ospf <process_id> area <area_number>`
	- configure an OSPF directly on an interface
	- alternate to `network` command
	- requires Interfac Config mode
	- no need for OSPF config mode
	- do `router ospf <process_id>` after the above command to enable OSPF on the R
- `R1(config-router)# passive-interface default`
	- configure ALL interfaces as OSPF passive interfaces
	- requires OSPF config mode
- `no passive-interface <interface_id>`
	- Once you configure all interfaces as passive, this command can be used to undo it for specific interfaces
- `R2(config-if)# ip ospf priority <0-255>`
	- manually configure opsf priority of an interface 
	- requires interface config mode
	- If the priority is set to 0, the R cannot be the DR/BDR for the subnet
- `R1(config-if)# ip ospf hello-interval <1-65535>`
	- change Hello timer
	- hello timer default = 10 seconds
	- reset to default with `no ip ospf hello-interval`
- `R1(config-if)# ip ospf dead-interval <1-65535>`
	- change Dead Timer
	- default = 40 seconds
	- reset to default with `no ip ospf dead-interval`
- `router-id a.b.c.d`
	- change router id
	- defaults to loopback addr ip OR physical addr IP
- `R1(config-if)# ip ospf authentication-key <password>`
	- sets the OSPF password
- `R1(config-if)# ip ospf authentication`
	- enables th previously set password
- `R1(config-if)# ip mtu <68-1500 bytes>`
	- change IP MTU (max transmission unit)
	- Default 1500 bytes
	- reset with `no ip mtu`


### HSRP
- `R1(config-if)# standby <group_number> ip <virtual_ip>`
	- confgures HSRP with that group number and virual ip
- `R1(config-if)# standby <group_number> priority <0-255>`
	- sets the priority of the interface
	- Higher priority gets preference to be Active
- `R1(config-if)# standby <group_number> preempt`
	- enables preemption for this Router
	- This Router will auto become Active if it ever goes down and comes back
- `R1(config-if)# standby version 2`
	- use version 2 of HSRP on this R
	- this must match the other Rs in the VLAN
- `R1# show standby`
	- shows Router's HSRP details


### IPv6

- `R1(config)# ipv6 unicast-routing`
	- enables ipv6 on the device
- `R1(config-if)# ipv6 enable`
	- enables  ipv6 on the interface
- `R1(config-if)# ipv6 address <ipv6_addr>`
	- address short forms are accepted
- `R1(config-if)# ipv6 address <network_addr> eui-64`
	- instructs the interface to eui-64 convert its MAC address to an IPv6 IP
- `R1(config-if)# ipv6 address <address>/<prefix-length> anycast`
	- configure an ipv6 anycast address
- `R1# show ipv6 int br`


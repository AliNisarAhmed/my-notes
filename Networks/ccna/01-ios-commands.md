
### Modes
1. `conf t`
	- Go to Global Config
2. `enable`
	- go from USER EXEC to PRIVILEGED EXEC
3. `interface <interface_name>`
	- Go to Interface config mode
4. `show ip interface brief`:
	- show interface, IP addr, and ports
5. `show interface <interface_number>`: 
	- e.g. `show interface gig0/0`
	- show details about an interface

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

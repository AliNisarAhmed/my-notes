# Protocols 

## Standards

IEEE 802.11  -> Wifi
ISO/IEC 7498 -> OSI Model


IEEE 802.1x  -> User Authentication
IEEE 802.1D -> STP (Spanning Tree P)
IEEE 802.1w -> Rapid STP
IEEE 802.1s -> Multiple STP

IEEE 802.3   -> Ethernet
IEEE 802.3i -> 10 Mbps Ethernet (10BASE-T)
IEEE 802.3u -> 100 Mbps FastEthernet (100BASE-T)
IEEE 802.3ab -> 1 Gbps Ethernet (1000BASE-T)
IEEE 802.3an -> 10 Gbps Ethernet (10GBASE-T)
IEEE 802.3z -> 1000BASE-LX
IEEE 802.3ae -> Fiber cables (10GBASE-SR, 10GBASE-LR, 10GBASE-ER)

IEEE 802.3ad -> Link Aggregation (Ethernet)
IEEE 802.3af -> PoE (Power over Ethernet) (15.4 Watts)
IEEE 802.3at -> PoE+ (25.5 Watts)

IEEE 802.1q -> Trunking Protocol

IEEE 802.3ad -> LACP

## Ports

| Protocol                      | Port      | Protocol purpose                                                                  |
|-------------------------------|-----------|-----------------------------------------------------------------------------------|
| FTP                           | 20, 21    | insecure file transfer                                                            |
| SSH                           | 22        | securely remote control another machine                                           |
| SFTP                          | 22        | secure FTP                                                                        |
| Telnet                        | 23        | insecurely remote control another machine (text based like ssh)                   |
| SMTP (Simple Mail Transfer)   | 25        | send emails over n/w                                                              |
| DNS                           | 53        | converts DN to IP addr                                                            |
| DHCP (Dynamic Host Control)   | 67, 68    | provides n/w params to clients, such as IPAddr, subnet mask, def g/w & DNS server |
| TFTP (Trivial File Transfer)  | 69        | insecure lightweight file transfer used for sending configs or n/w OS boot        |
| HTTP                          | 80        | insecure web browsing                                                             |
| POP3 (Post Office P v3)       | 110       | receive incoming emails (insecure)                                                |
| NTP                           | 123       | accurate time for clients on a n/w                                                |
| NetBIOS (n/w basic IO system  | 139       | file or printer sharing in Windows (auth)                                         |
| IMAP (Internet Mail App)      | 143       | newer method of retrieving emails, improves upon POP3                             |
| SNMP (Simple n/w mgmt)        | 161,162   | collect data abt n/w devices & monitor their status                               |
| LDAP (lightweight Dir Access) | 389       | provide directory service to n/w (address book, info on users/groups)             |
| HTTPS                         | 443       | secure web browsing                                                               |
| SMB (Server Msg Block)        | 445       | Windows file and printer sharing                                                  |
| SysLog                        | 514       | send logging data back to centralized server                                      |
| SMTP TLS                      | 587       | securely send emails                                                              |
| LDAPS                         | 636       | secure directory services                                                         |
| IMAPS                         | 993       | securely receive emails                                                           |
| POP3 over SSL                 | 995       | securely receive emails                                                           |
| SQL Server                    | 1433      | Used by MS SQL Server                                                             |
| SQLNet                        | 1521      | Used by Oracle DB                                                                 |
| MySQL                         | 3306      | Used by MySQL                                                                     |
| RDP                           | 3389      | GUI for remote control another client or server                                   |
| SIP (Session Initiation)      | 5060,5061 | initiate VoIP & video calls                                                       |



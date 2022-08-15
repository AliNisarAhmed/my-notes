# Packets and Frames

Packets and frames are small pieces of data that, when forming together, make a larger piece of information or message. 

However, they are two different things in the OSI model. 

A frame is at layer 2 - the data link layer, meaning there is no such information as IP addresses. Think of this as putting an envelope within an envelope and sending it away. The first envelope will be the packet that you mail, but once it is opened, the envelope within still exists and contains data (this is a frame).
- This process is called encapsulation

Packets are an efficient way of communicating data across networked devices. Because this data is exchanged in small pieces, there is less chance of bottlenecking occurring across a network than large messages being sent at once.

Packets have different structures that are dependant upon the type of packet that is being sent.

A packet using the Internet Protocol will have a set of headers that contain additional pieces of information to the data that is being sent across a network.
- Some notable headers: 
    - Time to Live TTL
    - Checksum
    - Source Address
    - Destination Address

## TCP - three way handshake

TCP packets contain various sections of information known as headers that are added from encapsulation.
- Source Port
- Destination port
- Source IP
- Destination IP
- Sequence Number
    - When a connection occurs, the first piece of data transmitted is given a random number.
- Acknowledgement Number
    - After a piece of data has been given a sequence number, the number for the next piece of data will have the sequence number + 1.
- checksum
- data
- flag
    - This header determines how the packet should be handled by either device during the handshake process. Specific flags will determine specific behaviours

3-way handshake messages
1. SYN
2. SYN/ACK
3. ACK
4. DATA
5. FIN
    - This packet is used to _cleanly (properly)_ close the connection after it has been complete.
6. RST
    - This packet abruptly ends all communication, this is last resort and indicates there was some problem during the process
    - For example, if the service or application is not working correctly, or the system has faults such as low resources.

Closing a connection
1. FIN
2. ACK/FIN
3. ACK

## UDP

UDP packets are much simpler than TCP packets and have fewer headers.
- TTL
- Source Address
- Destination Address
- Source Port
- Destination Port
- Data

## Ports

Ports can take a numerical value between 0-65535

- FTP - 21
- SSH - 22
- HTTP - 80
- HTTP - 443
- SMB - 445 (Server Message Block, files + share devices like printers)
- RDP - 3389

[Table of the 1024 common ports](http://www.vmaxx.net/techinfo/ports.htm)
# Networks

## The TCP/IP 5 layer Network Model

| #   | Layer                 | Protocol          | Protocol Data unit | Addressing   |
| --- | --------------------- | ----------------- | ------------------ | ------------ |
| 5   | Application           | HTTP, SMTP, FTP   | Messages           | n/a          |
| 4   | Transport             | TCP, UDP          | Segment            | Port numbers |
| 3   | Network               | IP                | Datagram           | IP Address   |
| 2   | Data Link / Nw Access | Ethernet, WiFi    | Frames             | MAC Address  |
| 1   | Physical              | 10 Base T, 802.11 | Bits               | n/a          |

---

## Network Switches vs Hubs

- A **hub** is a physical layer device that allows for connections from many computers at once.
- In a hub, all devices talk to ALL other devices all the time.
- This results in a lot of noise on the network callec **collision domain**.
- Hubs are mostly historical artifact today.
- A **network switch** is a level 2 (or data link layer) device (in 5 layer model).
- This means that a switch can actually inspect the contents of the Ethernet
  protocol data being sent around the network, determine which system
  the data is intended for and then only send that data to that one system.

---

## Routers

- A router is a device that knows how to forward data between independent networks.
- While a hub is a layer one device and a switch is a layer two device. A router operates at layer three, a network layer.
- A Router can inspect IP data to determine where to send things.
- Routers store internal tables containing information about how to route traffic
  between lots of different networks all over the world.
- The most common type of router you'll see is one for a home network, or
  a small office. These devices generally don't have very detailed routing tables.

---

## Ethernet

- Ethernet, the Data Link layer for wired connections, was first proposed in 1983.
- Ethernet solved the Data collision problems, present in hubs, by using a tecnique known as **Carrier Sense Multiple Access with Collision Detection**. (CSMA/CD)
- CSMA/CD is used to determine when the communications channel is clear, and when a device is free to transmit data.
- The way CSMA/CD works is that
  - if there's no data currently being transmitted on the network segment, a node will feel free to send data.
  - If it turns out that two or more computers end up trying to send data at the same time, the computers detect this collision and stop sending data.
  - Each device involved with the collision then waits a random interval of time before trying to send data again.
  - This random interval helps to prevent all the computers involved in the collision from colliding again the next time they try to transmit anything.

### MAC Addresses

- To Identify which node a transmission is actually meant for, we use **MAC addresses**
- A MAC address is a globally unique identifier attached to an individual network interface.
- It's a 48-bit number normally represented by six groupings of two hexadecimal numbers.
- The first three octets of a MAC address are known as the Organizationally Unique Identifier or OUI.
- These are assigned to individual hardware manufacturers by the IEEE or the Institute of Electrical and Electronics Engineers. This is a useful bit of information to keeping your back pocket because it means that you can always identify the manufacturer of a network interface purely by its MAC address.
- The last three octets of MAC address can be assigned in any way that the manufacturer would like with the condition that they only assign each possible address once to keep all MAC addresses globally unique.
- Ethernet uses MAC addresses to ensure that
  - the data it sends has both an address for the machine that sent the transmission, as well as the one that the transmission was intended for.
  - In this way, even on a network segment, acting as a single collision domain, each node on that network knows when traffic is intended for it.

### IP Addresses

- IP addresses are 32-bit long numbers made up of 4 octets, and each octet is normally described in decimal numbers.
- 8 bits of data, or a single octet,can represent all decimal numbers from 0 to 255.
- This format is known as dotted decimal notation.
- It's important to call out that IP addresses belong to the networks, not the devices attached to those networks. So your laptop will always have the same MAC address, no matter where you use it. But it'll have a different IP address assigned to it on different networks.
- IP address will be assigned to it automatically through a technology known as Dynamic Host Configuration Protocol.
- An IP address assigned this way is known as a dynamic IP address.
- The opposite of this is known as a static IP address, which must be configured on a node, manually.
- In most cases, static IP addresses are reserved for servers and
  network devices, while dynamic IP addresses are reserved for clients.
  But there are certainly situations where this might not be true.

### Classes

- IP addresses can be split into two sections, the network ID, and the host ID.
- There are three primary types of address classes.
  Class A, class B, and class C.
- Class A addresses are those where the first octet is used for the network ID,
  and the last three are used for the host ID.
- Class B addresses are where the first two octets are used for the network ID,
  and the second two, are used for the host ID.
- Class C addresses, are those where the first three octets are used for the network ID, and only the final octet is used for the host ID.

### Address Resolution Protocol (ARP)

- ARP is a protocol used to discover the hardware address of a node with a certain IP address.
- Once an IP datagram has been fully formed, it needs to be encapsulated inside an Ethernet frame. This means, that the transmitting device needs a destination MAC address to complete the Ethernet frame header.
- Almost all network connected devices retain local ARP table. An ARP table is just a list of IP addresses and the MAC addresses associated with them.
- Let's say we want to send some data to the IP address `10.20.30.40`. It might be the case that this destination doesn't have an entry in the ARP table.
- When this happens, the node that wants to send data sends a broadcast ARP message to the Mac broadcast address which is all Fs. These kinds of broadcast ARP messages are
  delivered to all computers on the local network.
- When the network interface that's been assigned an IP of 10.20.30.40 receives this ARP broadcast, it sends back what's known as an ARP response.
- This response message will contain the MAC address for the network interface in question.
- Now, the transmitting computer knows what MAC address to put in the destination hardware address field and the Ethernet frame is ready for delivery.
- It will also likely store this IP address in its local ARP table so that it won't have to send an ARP broadcast the next time he needs to communicate with this IP.
- ARP table entries generally expire after a short amount of time to ensure changes in the network are accounted for

### Subnetting

- Subnetting is the process of taking a large network and splitting it up into many individual smaller subnetworks or subnets.
  - address classes give us a way to break the total global IP space into discrete networks.
    If you want to communicate with the IP address 9.100.100.100, core routers on the Internet know that this IP belongs to the 9.0.0.0 class A network.
  - They then route the message to the gateway router responsible for the network by looking at the network ID. A gateway router specifically serves as the entry and exit path to a certain network. You can contrast this with core Internet routers, which might only speak to other core routers.
  - Once your packet gets to the gateway router for the 9.0.0.0 class A network, that router is now responsible for getting that data to the proper system by looking at the host ID.
  - This all makes sense until you remember that a single class A network contains 16,777,216 individual IPs.
  - That's just way too many devices to connect to the same router.
  - This is where subnetting comes in. With subnets, you can split your large network up into many smaller ones. These individual subnets will all have their own gateway routers serving as the ingress and egress point for each subnet.

### Subnet Masks and Subnet IDs

- Subnet Masks and Ids are to be considered for two cases, for Network Classes and CIDR. Below is for Network Classes.

- IP Address :: 9 - 100 - 100 - 100
- IP Address (Binary) :::: 0000 1001 . 0110 0100 . 0110 0100 . 0110 0100
- Subnet Mask :::::::::::: 1111 1111 . 1111 1111 . 1111 1111 . 0000 0000 (255.255.255.0) (24 1s 8 0s)

- Since `9` is the network class, the remaining bits after it and corrsponding to `1s` in subnet masks are subnet id, hence above `0110 0100 . 0110 0100`, the last octet is the host id
- Since we only have one octet for hosts, we have 2^8 hosts available, and out of those, the first `0000 0000` and `1111 1111` are reserved (latter for broadcast)

- Subnet mask like 255.255.255.224 translates to 27 `1s`, the shorthand can be `/27`, hence the above IP will be `9.100.100.100/27`.

### Problem with Network classes

- Address classes were the first attempt at splitting up the global Internet IP space.
- Subnetting was introduced when it became clear that address classes themselves weren't as efficient way of keeping everything organized.
- But as the Internet continued to grow, even traditional subnetting just couldn't keep up.
- With traditional subnetting and the address classes, the network ID is always either 8 bit for class A networks, 16 bit for class B networks, or 24 bit for class C networks.
- This means that there might only be 254 classing networks in existence, but it also means there are 2,970,152 potential class C networks.
- That's a lot of entries in a routing table.
- To top it all off, the sizing of these networks aren't always appropriate for the needs of most businesses
- 254 hosts in a class C network is too small for many use cases, but the 65,534 hosts available for use in a class B network is often way too large.
- Many companies ended up with various adjoining class C networks to meet their needs. That meant that routing tables ended up with a bunch of entries for a bunch of class C networks that were all actually being routed to the same place.

### CIDR (Classless Inter-domain routing)

- CIDR is an even more flexible approach to describing blocks of IP addresses.
- It expands on the concept of subnetting by using subnet masks to demarcate networks. To demarcate something means to set something off. (demarcation means where one network or system ends and another one begins)
- In our previous model, we relied on a network ID, subnet ID, and host ID to deliver an IP datagram to the correct location. With CIDR, the network ID and subnet ID are combined into one.
- CIDR basically just abandons the concept of address classes entirely,
  allowing an address to be defined by only two Individual IDs
- Earlier, network sizes were static. Think only class A, class B or, class C, and only subnets could be of different sizes.
- CIDR allows for networks themselves to be differing sizes. Before this, if a company needed more addresses than a single class C could provide, they need an entire second class C.
- With CIDR, they could combine that address space into one contiguous chunk with a net mask
  of /23 or 255.255.254.0.
- This means, that routers now only need to know one entry in their routing table to deliver traffic to these addresses instead of two.
- So, For Example, if a /24 network has two to the eight or 256 potential hosts, you really only have 256 minus two, or 254 available IPs to assign. If you need two networks of this size, you have a total of 254 plus 254 or 508 hosts. A single /23 network, on the other hand, is two to the nine or 512. 512 minus two, 510 hosts.

#### Routing Tables

Each Router stores a Routing table with at least the following four columns (with millions of rows)

- Destination N/w
  - This column would contain a row for each N/w the router know about, consists of usually a network IP and a net mask.
  - There usually also is a catcha-all entry that matches any IP address that does not have an explicit listing for.
- Net Hop
  - IP Address of the next router that should receive data intended for the destination networking question. OR this could just state that the network is directly connected and that there arent any additional hops.
- Total hops
  - The total hops needed to reach that router.
  - Thus, the router keeps trach of how far away that destination network is,
  - this number can change at any time.
- Interface
  - this is the anme of the interface on the router, which the router should forward the traffic that matches this row's network destination.

#### Routing Protocols

These protocols help routers keep up to date information about the routers next to them or far away.

There are two main Categories

- Interior gateway protocols
  - Distance Vector routing
  - Link State Routing
- Exterior Gateway protocols

##### Interior gateway protocol

- Used by routers to share information within a single autonomous system
  - an autonomous system is a collection of networks that all fall under the control of a single network operator. e.g. large corporation or a nation wide ISP
- In contrast, exterior gateway protocols are used for exchange of info b/w independent autonomous systems.

##### Distance Vector protocol

- A now outdated system, in which a router takes its routing table and shares it with every router immediately adjacent to it.
- since a list is known as vector in CS, that's why this sharing of Routing tables (list) is so named.
- This protocol does not allow for a router to have much information about the state of the world outside of their own direct neighbours.
- Because of this a router might be slow to react to changes in the network that happen far away from it.

##### Link State Protocol

- Here, each router advertises the state of the link of each of its interfaces.
- These interfaces can be connected to other routers or they could be direct connections to networks.
- The information about each router is propagated to every other router on the autonomous system.
- This means that every router on the system knows every detail about every other router in the system.
- Each router then uses this info and runs complicated algos against it to determine the best path to any destination network.
- Link state protocols require both more memory in order to hold all of this data
  and also much more processing power. This is because it has to run algorithms against this data in order to determine the quickest path to update the routing tables.
- As computer hardware has become more powerful and cheaper over the years, link state protocols have mostly made distance vector protocols outdated.

##### Exterior Gateway Protocols

- are used to communicate data between routers representing the edges of an autonomous system.
- Routers use this protocol when they need to share information across different organizations.
- the **IANA**, or Internet Assigned Numbers Authority, is a non-profit organization that helps manage things like IP Address allocation. Along with managing IP Addresses, the IANA is also responsible for **ASN** or Autonomous System Number allocation
- ANSs are 32-bit numbers just like IP addresses, assigned to individual autonomous systems.
- They're normally referred to as a single decimal number.
  - Because ASNs dont need to change
  - They arent meant to be human readable often
- AS19604 for example, belongs to IBM.

##### Examples of above protocols

- The most common distance vector protocols are
  - RIP (Routing Information protocol)
  - EIGRP (Enhanced Interior Gateway Routing Protocol)
- The most common Link state protocol is
  - OSPF (Open Shortest Path First)
- There's only one exterior gateway protocol, called BGP (Border Gateway Protocol)

#### Non-Routable Address Space

- RFC 1918 outlined a number of networks that would be defined as non-routable addresses.
- Non-routable address space are a range of IPs set aside for use by anyone that cannot be routed to.
- No every computer connected to the internet needs to communicate with every other computer, so these address space allow for nodes on such a network to commmunicate with each other but no gateway router will attempt to forward traffic to this type of network.
- RFC 1918 defined three ranges of IP addresses that will never be routed anywhere by co-routers. That means that they belong to no one and that anyone can use them. In fact, since they are separated from the way traffic moves across the internet, there's no limiting to how many people might use these addresses for their internal networks.
- The primary range of these non-routable address space are
  - 10.0.0.0/8
  - 172.16.0.0/12
  - 192.168.0.0/16

---

## Transport Layer

- Multiplexing in the transport layer means that nodes on the network have the ability to direct traffic toward many different receiving services.
- De-multiplexing is the same concept, just at the receving end. It;s taking traffic that's all aimed at the same node and delivering it to the proper receiving service.
- The Transport layer handles Multiplexing and demultiplexing through ports.
- A port is a 16-bit number that's used to direct traffic to specific services running on a networked computer.
- Different network services run while listening on specific ports for
  incoming requests.
  - Ports are normally denoted with a colon after the IP address.
  - For example, the traditional port for HTTP or unencrypted web traffic is port 80.
    If we want tor request the webpage from a web server running on a computer listening
    on IP 10.1.1.100, the traffic would be directed to port 80 on that computer.
  - So the full IP and port in this scenario could be described as 10.1.1.100:80.
    When written this way, it's known as a socket address or socket number.
- The same device might also be running an FTP or file transfer protocol server.
  FTP is an older method used for transferring files from one computer
  to another, but you still see it in use today.
  - FTP traditionally listens on port 21, so if you wanted to establish a connection to
    an FTP server running on the same IP that our example web server was running on,
    you direct traffic to 10.1.1.100 port 21

### TCP Segment

![Tcp Segment](../images/tcp-segment.PNG)

Just like how an Ethernet frame encapsulates an IP datagram,
an IP datagram encapsulates a TCP segment.
Remember that an Ethernet frame has a payload section
which is really just the entire contents of an IP datagram.
Remember also that an IP datagram has
a payload section and this is made up of what's known as a TCP segment.
A TCP segment is made up of a TCP header and a data section.
This data section, as you might guess,
is just another payload area for where the application layer places its data

- Destination Port: The destination port is the port of the service the traffic is intended for.
- A source port is a high numbered port chosen from a special section of ports known as ephemeral ports.
  - a source port is needed so that when the web server replies,
    the computer making the original request can send
    this data to the program that was actually requesting it
- Sequence Number: a 32-bit number that's used to keep track of where in a sequence of TCP segments this one is expected to be.
  - You might remember that lower on our protocol stack, there are limits to the total size of what we send across the wire.
  - In Ethernet frame, it's usually limited in size to 1,518 bytes, but we usually need to send way more data than that.
  - At the transport layer, TCP splits all of this data up into many segments.
  - The sequence number in a header is used to keep track of which segment out of many this particular segment might be.
- The acknowledgment number is the number of the next expected segment.
  - In very simple language, a sequence number of one and an acknowledgement number of
    two could be read as this is segment one,
    expect segment two next.
- Data offset field: four-bit number that communicates how long the TCP header for this segment is
- six bits that are reserved for the six TCP control flags
- A TCP window specifies the range of sequence numbers that might be sent before an acknowledgement is required.
  - TCP is a protocol that's super reliant on acknowledgements.
    This is done in order to make sure that all expected data is actually being
    received and that the sending device doesn't
    waste time sending data that isn't being received.
- The next field is a 16-bit checksum.
  - It operates just like the checksum fields at the IP and Ethernet level.
- Urgent pointer fields and options field are rarely used.
- padding is a sequence of zeroes to ensure that the payload section begins at the expected location

#### TCP Control Flags

- As a protocol, TCP establishes connections used to send long chains of segments of data.
  - contrasted with the protocols that are lower in the networking model. These include IP and Ethernet, which just send individual packets of data.
- The way TCP establishes a connection, is through the use of different TCP control flags,
  used in a very specific order.
- There are 6 TCP Control Flags
  1. URG (Urgent)
     - A value of one here indicates that the segment is considered urgent and that the urgent pointer field has more data about this. (Not used often)
  2. ACK, (Acknowledge).
     - A value of one in this field means that the acknowledgment number field should be examined.
  3. PSH (Push)
     - This means, that the transmitting device wants the receiving device to push currently buffered data to the application on the receiving end as soon as possible.
     - A buffer is a computing technique, where a certain amount of data is held somewhere,
       before being sent somewhere else.
     - In terms of TCP, it's used to send large chunks of data more efficiently.
     - By keeping some amount of data in a buffer, TCP can deliver more meaningful chunks of data to the program waiting for it. But in some cases, you might be sending a very small amount of information, that you need the listening program to respond to immediately.This is what the push flag does.
  4. RST (Reset)
     - This means, that one of the sides in a TCP connection hasn't been able to properly recover from a series of missing or malformed segments.
     - It's a way for one of the partners in a TCP connection to basically say, "Wait, I can't put together what you mean, let's start over from scratch."
  5. SYN (Synchonize)
     - It's used when first establishing a TCP connection and make sure
       the receiving end knows to examine the sequence number field
  6. FIN (Finish)
     - When this flag is set to one, it means the transmitting computer doesn't have
       any more data to send and the connection can be closed.

#### Three way Handshake (to initiate a connection)

<details>
  <summary>Three way Handshake diagram</summary>

![Three way handshake](../images/syn-ack-syn.PNG)

</details>

#### Four way Handshake (to close a connection)

<details>
  <summary>Four way Handshake diagram</summary>

![Four way handshake](../images/four-way.PNG)

</details>

### TCP Sockets and Socket States

- A socket is the instantiation of an endpoint in a potential TCP connection. An instantiation is the actual implementation of something defined elsewhere.
- TCP sockets require actual programs to instantiate them.
- You can contrast this with a port which is more of a virtual descriptive thing.
- In other words, you can send traffic to any port you want, but you're only going to get a response if a program has opened a socket on that port.
- TCP sockets can exist in lots of states

  - Listen
    - Listen means that a TCP socket is ready and listening for incoming connections.
      You'd see this on the server side only.
  - SYN_SENT.
    - This means that a synchronization request has been sent,
      but the connection hasn't been established yet.
      You'd see this on the client side only
  - SYN_RECEIVED.
    - This means that a socket previously in a listener state,
      has received a synchronization request and sent a SYN_ACK back.
      But it hasn't received the final ACK from the client yet.
      You'd see this on the server side only.
  - ESTABLISHED. This means that the TCP connection is in working order, and both sides are free to send each other data. You'd see this state on both the client and server sides of the connection.
  - FIN_WAIT. This means that a FIN has been sent,
    but the corresponding ACK from the other end hasn't been received yet.
  - CLOSE_WAIT. This means that the connection has been closed at the TCP layer, but that the application that opened the socket hasn't released its hold on the socket yet.
  - CLOSED. This means that the connection has been fully terminated,
    and that no further communication is possible.
    There are other TCP socket states that exist.

- Additionally, socket states and their names, can vary from operating system to operating system. That's because they exist outside of the scope of the definition of TCP itself.

### Connection Oriented (TCP) vs Connectionless Protocols (UDP)

- A connection-oriented protocol is one that establishes a connection, and uses this to ensure that all data has been properly transmitted.
- A connection at the transport layer implies that every segment of data sent is acknowledged.
  This way, both ends of the connection always know which bits of
  data have definitely been delivered to the other side and which haven't
- Connection-oriented protocols are important because the Internet is a vast and busy place,
  and lots of things could go wrong while trying to get data from point A to point B.
- Anything can go wrong when transmitting packets . Connection-oriented protocols like TCP protect against this by forming connections and through the constant stream of acknowledgements.
- Our protocols at lower levels of our network model like IP and Ethernet do use checksums to ensure that all the data they received was correct.And they do not resend data. Re-sending of data is decided by TCP layer.
- At the IP or Ethernet level, if a checksum doesn't compute, all of that data is just discarded.
- It's up to TCP to determine when to resend this data since TCP expects an ACK for every bit of data it sends, it's in the best position to know what data successfully got
  delivered and it can make the decision to resend a segment if needed.
- This is another reason why sequence numbers are so important. While TCP will generally send all segments in sequential order, they may not always arrive in that order.
- If some of the segments had to be resent due to errors at lower layers, it doesn't matter if they arrive slightly out of order. This is because sequence numbers allow for all of the data to be put back together in the right order. It's pretty handy.
- there's a lot of overhead with connection-oriented protocols like TCP. You have to establish the connection, you have to send a stream of constant streams of acknowledgements,
  you have to tear the connection down at the end. That all accounts for a lot of extra traffic.
  While this is important traffic, it's really only useful if you absolutely positively have to be sure your data reaches its destination.
- You can contrast this with connectionless protocols. The most common of these is known as UDP, or **User Datagram Protocol**
- Unlike TCP, UDP doesn't rely onconnections and it doesn't even support the concept of an acknowledgement.
- With UDP, you just set a destination port and send the packet. This is useful for messages that aren't super important. A great example of UDP is streaming video. Where even of one or two frames are missed here or there, it doesn't really matter.
- By getting rid of all the overhead of TCP, you might actually be able to send higher quality video with UDP.

#### System Ports vs Ephemeral Ports

- https://www.youtube.com/watch?v=mykX2YONRwE

Transportation layer protocols use a concept of ports and multiplexing/demultiplexing to deliver data to individual services listening on network nodes. These ports are represented by a single 16-bit number, meaning that they can represent the numbers 0-65535.

This range has been split up by the IANA (**Internet Assigned Numbers Authority**) into independent sections:

Port 0 isn’t in use for network traffic, but it’s sometimes used in communications taking place between different programs on the same computer.

Ports 1-1023 are referred to as **system ports**, or sometimes as **“well-known ports.”** These ports represent the official ports for most well-known network services. HTTP normally communicates over port 80, while FTP usually communicates over port 21. In most operating systems, administrator-level access is needed to start a program that listens on a system port.

Ports 1024-49151 are known as **registered ports**. These ports are used for lots of other network services that might not be quite as common as the ones that are on system ports. A good example of a registered port is 3306, which is the port that many databases listen on. Registered ports are sometimes officially registered and acknowledged by the IANA, but not always. On most operating systems, any user of any access level can start a program listening on a registered port.

Finally, we have ports 49152-65535. These are known as **private or ephemeral ports**. Ephemeral ports can’t be registered with the IANA and are generally used for establishing outbound connections. You should remember that all TCP traffic uses a destination port and a source port. When a client wants to communicate with a server, the client will be assigned an ephemeral port to be used for just that one connection, while the server listens on a static system or registered port.

Not all operating systems follow the ephemeral port recommendations of the IANA. In this lesson, we’ll continue to assume that the ephemeral ports used for outbound connections consist of the ports 49152 through 65535. But it’s important to know that this exact range can vary depending on the platform you’re working on. Sometimes portions of the registered ports range are used, but no modern operating system will ever use a system port for outbound communication

### Firewalls

- A firewall is just a device that blocks traffic that meets certain criteria.
  Firewalls are a critical concept to keeping a network secure since they
  are the primary way you can stop traffic you don't want from entering a network.
- Firewalls can actually operate at lots of different layers of the network.
  There are firewalls that can perform inspection of application layer traffic,
  and firewalls that primarily deal with blocking ranges of IP addresses.
- Firewalls that operate at the transportation layer
  will generally have a configuration that enables them to block traffic
  to certain ports while allowing traffic to other ports
- Firewalls are sometimes independent network devices, but
  it's really better to think of them as a program that can run anywhere.
  For many companies and almost all home users, the functionality of a router and
  a firewall is performed by the same device.
  And firewalls can run on individual hosts instead of being a network device.
  All major modern operating systems have firewall functionality built-in.

---

## Application Layer

- There are many application protocols, but for each application type, the protocol is still standardized.
  - The same is true for most other classes of application.
    You might have dozens of choices for an FTP client, but
    they all need to speak the FTP protocol in the same way.
- In the more commonly used in academia, the OSI (Open Systems Iterconnection) model, there are two more layers between application layer and transport layer, namely, Session and Presentation Layer
  - Session: sits above transport, The concept of a session layer is that it's responsible for
    things like facilitating the communication between actual applications and
    the transport layer.
    It's the part of the operating system that takes the application layer data
    that's been unencapsulated from all the layers below it, and
    hands it off to the next layer in the OSI model, the presentation layer.
  - Presentation layer: The presentation layer is responsible for
    making sure that the unencapsulated application layer data
    is actually able to be understood by the application in question.
    This is the part of an operating system that might handle encryption or
    compression of data.

#### NOTE: Watch the video to see how all the layers work together to perform the task of sending data over a network.

---

## Domain Name Syetem (DNS)

- For better human readability, we need to translate the IP addresses of web servers e.g. 192.168.1.100 to e.g. www.weather.com, this is the job of DNS.
- A domain name is just the term we use for something that can be resolved by DNS.

#### Steps involved in Name Resolution

- At its most basic, DNS is a system that converts domain names into IP addresses.
- This process of using DNS to turn a domain name into an IP address is known as **name resolution**.
- The first thing that's important to know is that DNS servers, are one of the things that need to be specifically configured at a node on a network.
- a computer, in order to operate on a modern network, need to have these four things configured, MAC Address, IP Address, subnet mask, gateway for a host, and DNS Server.
- A computer, though, can operate fine without a DNS Server
- There are five primary types of DNS Servers
  1. Caching Name servers
  2. Recursive Name servers
  3. Root name servers
  4. TLD Name servers
  5. Authoritative Name servers
- Any given Name server can perform many of the above roles at once.
- Caching and Recurssive name servers are usually provided by the local ISP.
- If the name server does know the IP of a domain name, it will perform the full search, and then that server is known as recursive name servers.
- To avoid doing that, name servers cache the domain names, hence these servers are called caching name servers.
- All domain names in the global DNS system have a TTL (time to live.)
  - This is a value in seconds, that can be configured by the owner of a domain name for how long a name server is allowed to cache in entry before it should discard it and perform a full resolution again.
  - Used to be a day, but now are usually few hours to a few minutes.
- Recursive Name Resolution
  - The first step always is to contact a Root name Server.
  - There are "13" total Root name servers and they're responsible for directing queries toward the approporiate TLD Name servers.
  - These Root name servers are now globally distributed via anycast.
  - **Anycast** is a technique that's used to route traffic to different destinations depending on factors like location, congestion, or link health. - How does anycast routing do this? - Typically, a single IP address is advertised across multiple regions. \
    - Rather than using a simple DNS query that resolves back to a single IP address that only exists in one location, anycast routing allows the network infrastructure to intelligently determine where a request is coming from and route the customer to the closest advertised
      region. - This routing allows your customers to connect to your web application more quickly and provides a better overall customer experience.
  - The Root servers respond to a DNS lookup with the name of the TLD Name server that should be contacted next.
- TLD (Top Level Domain) Name Servers
  - TLD is the last part of any domain name, e.g. dot com part of www.facebook.com
  - For each TLD in existence, there is a TLD Name server
  - The TLD Name servers respond again with a redirect, this time informing the computer performing the name lookup with what Authoritative name server to contact.
- Authoritative Name Server
  - responsible for the last two parts of any domain name which is the resolution at which a single organization may be responsible for DNS Lookup.
  - e.g. for www.weather.com, the TLD name server would point a lookup at the authoritative server for Weather.com, which would likely be controlled by the Weather Channel, the organization that itself runs the site.
- This strict hierarchy is very important to the stability of the internet, making sure that all full DNS resolutions go through a strictly regulated and controlled series of lookups to get the correct response. This is the best way to protect against maliciious parties redirecting traffic.
- Out local computer (phone or desktop) will generally have its own temporary DNS cache as well, that way, it does not have to bother its local name server for every TCP connection either.

### DNS and UDP

- DNS is a great example of an application layer that uses UDP for the transport layer instead of TCP.
- UDP is connectionless, this means there is no setup and teardown of a connection like TCP.
- So much less traffic needs to be transmitted overall.
- a typical recursive name lookup using TCP will require 44 packets, compared to just 8 packets with UDP, demonstrating why protocols like UDP exist in addition to the more robust TCP.
- If an error occurs, the DNS resolver just asks again if it doesnt't get a response from the name server.
- However, DNS lookup over TCP does exist and is in use all over the world
  - the reason being that as the web has gotten more complex, it's no longer the case that all DNS lookup responses can fit in a single UDP datagram.
  - In these situations, a DNS name server would respond with a packet explaining that the response is too large
  - the DNS client would then establish a TCP connection in order to perform the lookup.

### Resource Record Type

- DNS in practice operates with a set of defined resource record types.
- These allow for different kinds of resolutions to take place.
- There are dozens of different record types defined, but a lot of them only serve a very specialized purpose.
- The most common resource record is **A record**
  - An A record is used to point a certain domain name at a certain IPv4 IP address.
  - usually a single domain name can have single A record, but multiple records are possible two.
  - the latter allows for a technique known as **DNS round robin** to be used to balance traffic across multiple IPs.
  - Round robin is a concept that involves iterating over a list of items one by one in an orderly fashion, with the hope that this ensures a fairly equal balance of each entry on the list that's selected.
    - So let's say that Microsoft has 4 IP addresses registered for www.microsoft.com,
    - for the first client, they may send IP address in the order A, B, C, D (where the client is intended to go to the address A)
    - but for the next client, the response would be B, C, D, A
    - This pattern would continue, cycling through all the A records configured
- **Quad A** record is very similar to an A record except that it returns an IPv6 address.
- A **CNAME record** is used to redirect traffic from one domain to another.
  - CNAME stands for Canonical name
  - e.g. google.com points to www.google.com
  - CNAMEs are really useful because they ensure you only have to change the canonical IP address of a server in one place e.g. www.google.com (the rest of the domain names can just point to this one)
- **MX Record** stands for mail exchange
  - this resource record is used in order to deliver e-mail to the correct server.
  - Many companies run their web and mail servers on different machines with different IPs, so the MX record makes it easy to ensure that email gets delivered to a company's mail server, while other traffic like web traffic would get delivered to their web server
- **SRV Record**
  - stands for service record.
  - an SRV record can be defined to return the specifics of many different service types.
  - For example, SRV records are often used to return the records of services like CalDAV, which has a calendar and scheduling service.
- **TXT** stands for text and was originally intended to be used only for associating some descriptive text with a domain name for human consumption.
  - The idea was that, you could leave notes or messages that humans could discover and read to learn more about arbitrary specifics of your network.
  - This text record is often used to communicate configuration preferences about network services that
    you've entrusted other organizations to handle for your domain.

### Anatomy of a Domain name

for `www.google.com`

- `.com` is Top level Domain
  - There are only a few TLDs defined
  - there are country specific TLDs
  - TLDs are handled by a non-profit **ICANN** (Internet Coporation for Assigned Names and Numbers)
  - ICANN is a sister organization to **IANA**, and together they help define and control both the global IP spaces, along with the global DNS system.
- `google` is known as domain
  - Domains are used to demarcate where control moves from a TLD name server, to an authoritative name server.
  - This is typically under the control of an independent organization, or someone outside of ICANN.
  - Domains can be registered and chosen by any individual or company, but they must all end in one of the predefined TLDs.
- `www` is known as subdomain (or hostname if it's only assigned to one host)
- When we combine all the three parts, we get **FQDN** (Fully qualified domain name)
- While it costs money to officially register a domain with a registrar, subdomains can be freely chosen and
  assigned by anyone who controls such a registered domain.
- A **registrar** is just a company that has an agreement with ICANN to sell unregistered domain names.
- Technically you can have lots of subdomain names, for example `host.sub.sub.subdomain.domain.com` can be completely valid. Although you rarely see fully qualified domain names with that many levels.
- DNS can technically support up to 127 levels of domain in total for a single fully qualified domain name.
- There are some other restrictions in place for how your domain name can be specified.
  - Each individual section can only be 63 characters long and
  - a complete FQDN is limited to a total of 255 characters

### DNS Zones in Authoritative Name servers

- Authoritative name servers are responsible for responding to name resolution requests for specific domains,
  but they do more than that.
- An authoritative name server is responsible for a specific DNS zone.
- DNS zones are a hierarchical concept.
- The root name servers are responsible for the root zone.
- Each TLD name server is responsible for the zone covering its specific TLD,
  and what we refer to as authoritative name servers are
  responsible for some even finer-grained zones underneath that.
- The root and TLD name servers are actually just authoritative name servers, too.
- It's just that the zones that they're authoritative for are special cases.
- It should be noted that zones don't overlap.
  - For example, the administrative authority of the TLD name server for the `.com` TLD doesn't encompass the `google.com` domain.
  - Instead, it ends at the authoritative server responsible for google.com.
- The purpose of DNS zones is to allow for easier control over multiple levels of a domain.
- As the number of resource records in a single domain increases, it becomes more of a headache to manage them all. Network administrators can ease this pain by splitting up their configurations into multiple zones.
  - Example
  - Let's imagine a large company that owns the domain largecompany.com. This company has offices in Los Angeles, Paris, and Shanghai, very cosmopolitan.
    Let's say each office has around 200 people with their own uniquely named desktop computer.
  - This would be 600 A records to keep track of if it was all configured as a single zone. What the company could do, instead, is split up each office into their own zone. So now, we could have la.largecompany.com, pa.largecompany.com, and sh.largecompany.com as subdomains, each with their own DNS zones.
  - A total of four authoritative name servers would now be required for the setup, one for largecompany.com and one for each of the subdomains.
- Zones are configured through what are known as zone files, simple configuration files that declare all resource records for a particular zone

### DHCP (Dynamic Host Configuration Protocol)

- Managing hosts on a network can be a daunting and time consuming task.
  - Every single computer on a modern TCP/IP based network needs to have at least four things to specifically configured.
    - An IP Address,
    - the subnet mask for the local network,
    - a primary gateway,
    - and a name server.
  - On their own, these four things don't seem like much, but when you have to configure them on hundreds of machines, it becomes super tedious. Out of these four things, three are likely the same on just about every node on the network, the subnet mask, the primary gateway, and DNS server. The IP needs to be different on every host.
- That could require a lot of tricky configuration work, and this is where DHCP, or Dynamic Host Configuration Protocol, comes into play.
- DHCP is an application layer protocol that automates the configuration process of hosts on a network.
  - With DHCP, a machine can query a DHCP server when the computer connects to the network and receive all the network configuration in one go.
  - Not only does DHCP reduce the administrative overhead of having to configure lots of network devices on a single network, it also helps address the problem of having to choose what IP to assign to what machine.
- Using DHCP, you can configure a range of IP addresses that's set aside for client devices that require fixed static IP, like gateways and DNS Servers.
- This ensures that any of these devices can obtain an IP address when they need one, but solves the problem of having to maintain a list of every node on the network and its corresponding IP.
- DCHP can operate in one of these standard ways
  - Dynamic Allocation
    - A range of IP addresses is set aside for client devices and one of these IPs is issued to these devices when they request one
    - Under a dynamic allocation, the IP of a computer could be different, almost every time it connects to the network.
  - Automatic Allocation
    - very similar to dynamic allocation, in that a range of IP addresses is set aside for assignment purposes.
    - The main difference here is that the DHCP server is asked to keep track of which IPs it's assigned to certain devices in the past.
    - Using this information, the DHCP server will assign the same IP to the same machine each time, if possible.
  - Fixed Allocation
    - requires a manually specified list of MAC address and the corresponding IPs.
    - When a computer requests an IP, the DHCP server looks for its MAC address in a table, and assigns the IP that corresponds to that MAC address.
    - If the MAC address isn't found, the DHCP server might fall back to automatic or dynamic allocation, or it might refuse to assign an IP altogether.
    - This can be used as a security measure to ensure that only devices that have had their MAC address specifically configured at the DHCP server will ever be able to obtain an IP and communicate on the network.

#### DHCP Discovery (DHCP in action)

- The process by which a client configured to use DHCP attempts to get network configuration information is known as DHCP discovery.
- DHCP Discovery has for steps
  - Server Discovery
    - The DHCP Client sends a DHCP DISCOVER message out onto the network.
    - Since the machine doesn't have an IP and it doesn't know the IP of the DHCP server, a specially crafted broadcast message is formed instead.
    - DHCP listens on UDP port 67 and DHCP discovery messages are always sent from UDP port 68.
    - So the DHCPDISCOVER message is encapsulated in a UDP datagram with a destination port of 67 and a source port of 68.
    - This is then encapsulated inside of an IP datagram with a destination IP of 255.255.255.255, and a source IP of 0.0.0.0.
    - This broadcast message would get delivered to every node on the local area network. And if a DHCP server is present, it would receive this message.
  - Next the DHCP server would examine its own configuration and would make a decision on what, if any, IP address to offer to the client.
    - This would depend on if it's configured to run with dynamic, automatic or fixed address allocation.
    - The response would be sent as a DHCPOFFER message with a destination port of 68, a source port of 67, a destination broadcast IP of 255.255.255.255, and its actual IP as the source.
    - Since the DHCP offer is also a broadcast, it would reach every machine on the network.
    - The original client would recognize that this message was intended for itself. This is because the DHCPOFFER has the field that specifies the MAC address of the client that sent the DHCPDISCOVER message.
  - the DHCP client would respond to the DHCPOFFER message with a DHCPREQUEST message.
    - This message essentially says, yes, I would like to have an IP that you offer to me.
    - Since the IP hasn't been assigned yet, this is again sent from an IP of 0.0.0.0, and to the broadcast IP of 255.255.255.255.
  - Finally, the DHCP server receives the DHCPREQUEST message and responds with a DHCPACK or DHCP acknowledgement message.
    - This message is again sent to a broadcast IP of 255.255.255.255, and with a source IP corresponding to the actual IP of the DHCP server.
    - Again, the DHCP client would recognize that this message was intended for itself by inclusion of its MAC address in one of the message fields.
    - The networking stack on the client computer can now use the configuration information presented to it by the DHCP server to set up its own network layer configuration.
- At this stage, the computer that's acting as the DHCP client should have all the information it needs to operate in a full fledged manner on the network it's connected to.
- All of this configuration is known as **DHCP lease** as it includes an expiration time.
- A DHCP lease might last for days or only for a short amount of time.
- Once a lease has expired, the DHCP client would need to negotiate a new lease by performing the entire DHCP discovery process all over again.
- A client can also release its lease to the DHCP server, which it would do when it disconnects from the network.
- This would allow the DHCP server to return the IP address that was assigned to its pool of available IPs.

---

## Network Address Translation (NAT)

- It's a technique rather than a standard, with OS based implementation.
- NAT takes one IP Address and translates/maps it onto another (usually host to router)
- NAT allows networks to use non-routable address space for their internal devices.
- At its most basic level NAT, is a technology that allows a gateway usually a router or a
  firewall to rewrite the source IP of an outgoing IP datagram, while retaining the original IP in order to rewrite it into the response.
  - So, lets say a computer A wants to talk to a Computer B on a different network, via a Router R.
  - whaat R normally does is inspect the contents of an IP Datagram, decrement TTL, recalculate the checksum and forward the rest of the data on the network layer without touching it.
  - if NAT is enabled, R will also re-write the IP address to its own, thus hiding the IP of the source.
  - This is known as **_IP masquerading_**

### NAT and the Transport layer

- With one to many NAT, we've talked about how hundreds even thousands of computers can all have
  their outbound traffic translated via NAT to a single IP. This is pretty easy to understand when the traffic is outbound, but a little more complicated once return traffic is involved.
- Several Additional techniques need to be employed to indentify the originator of a request after NAT.
- One simple way is Port Preservation

#### Port Preservation

- Port preservation is a technique where the source port chosen by a client,
  is the same port used by the router.
- Remember that outbound connections choose a source port at random, from the ephemeral ports or the ports in the range 49,152 through 65,535
- In the simplest setup, a router setup to NAT outbound traffic, will just keep track of what this source port is, and use that to direct traffic back to the right computer.
- Even with how large the set of ephemeral ports is,it's still possible for two different computers on a network to both choose the same source port around the same time. When this happens, the router normally selects an unused port at random to use instead.

#### Port Forwarding

- Port forwarding is a technique where a specific destination ports can be configured to always be delivered to specific nodes.
- This technique allows for complete IP masquerading, while still having services that can respond to incoming traffic.
- Let's say there's a web server configured with an IP of 10.1.1.5. With port forwarding, no one would even have to know this IP. Prospective web clients would only have to know about the external IP of the router. Let's say it's 192.168.1.1. Any traffic directed at port 80 on 192.168.1.1, would get automatically forwarded to 10.1.1.5.

**_NOTE_**: Read the answer to understand port forwarding [link](https://superuser.com/questions/284051/what-is-port-forwarding-and-what-is-it-used-for)

### IPv4 Exhaustion

- The IANA has been in charge of distributing IP addresses since 1988. Since that time, the internet has expanded at an incredible rate. The 4.2 billion possible IPv4 addresses have been predicted to run out for a long time and they almost have.
- For some time now, the IANA has primarily been responsible with assigning address blocks to the five **Regional Internet Registries** or **RIRs**.
  - AFRINIC: Africa
  - ARIN: US, Canada, parts of the carribean
  - APNIC: Asia, Australia, New Zealand and Pacific Islands
  - LACNIC: Central and South America
  - RIPE: Europe, Russia, Middle East and portions of Central Asia.
- These five RIRs have been responsible for assigning IP address blocks to organizations within their geographic areas and most have already run out.
- This is of course, a major crisis for the internet.
- IPv6 will eventually resolve these problems

* For now, we wanted to continue to grow and we want more people and devices to connect to it but without IP addresses to assign, a workaround is needed.
* This is enabled by NAT and Non-routable address space.
* Non-routable address space was defined in RFC1918 and consists of several different IP ranges that anyone can use.
* An unlimited number of networks can use non-routable address space internally because internet routers won't forward traffic to it.
* This means there's never any global collision of IP addresses when people use those address spaces.
* Non-routable address space is largely usable today
  because of technologies like NAT.
* With NAT, you can have hundreds even thousands of machines using non-routable address space.
* Yet, with just a single public IP, all those computers can still send traffic to and receive traffic from the internet.
* All you need is one single IPv4 address and via NAT, a router with that IP can represent lots and lots of computers behind it.
* It's not a perfect solution, but until IPv6 becomes more globally available, non-routable address space and NAT will have to do

---

## Virtual Private Networks (VPNs)

- Virtual Private Networks or VPNs, are a technology that allows for the extension of a private or local network, to a host that might not work on that same local network.
- When establishing a VPN connection, you might also say that a VPN tunnel has been established.
- Most VPNs work by using the payload section of the transport layer to carry an encrypted payload that actually contains an entire second set of packets, The network, the transport, and the application layers of a packet intended to traverse the remote network.
- Basically, this payload is carried to the VPNs endpoint, where all the other layers are stripped away and discarded. Then, the payload is unencrypted, leaving the VPN server with the top three layers of a new packet.
- This gets encapsulated with the proper data link layer information, and sent out across the network. This process is completed in the inverse, in the opposite direction.
- PNs, usually requires strict authentication procedures in order to ensure that they can only be connected to by computers and users authorized to do so.
- In fact, VPNs were one of the first technologies where two-factor authentication became common.

### Proxy Services

- A proxy service is a server that acts on behalf of a client in order to access another service.
- Proxies sit between clients and other servers, providing some additional benefit, anonymity,
  security, content filtering, increased performance, a couple other things.
- Proxy is a concept, there are tons of specific implementations. for example
  - Web Proxy
    - specifically built for web traffic.
    - Many years ago, when most Internet connections were much slower than they are today, lots of organizations used web proxies for increased performance.
    - Using a web proxy, an organization would direct all web traffic through it, allowing the proxy server itself to actually retrieve the webpage data from the Internet.
    - It would then cache this data. This way, if someone else requested the same webpage, it could just return the cached data instead of having to retrieve the fresh copy every time.
    - A more common use of a web proxy today might be to prevent someone from accessing sites, like Twitter, during office hours, for example.
  - Reverse Proxy
    - A reverse proxy is a service that might appear to be a single server to external clients, but actually represents many servers living behind it. A good example of this is how lots of popular websites are architected today.
    - A reverse proxy, in situation where one web server is not enough to handle load and we need multiple servers, could act as a single front-end for many web servers living behind it.
    - From the clients' perspective, it looks like they're all connected to the same server.
    - But behind the scenes, this reverse proxy server is actually distributing these incoming requests to lots of different physical servers. Much like the concept of DNS Round Robin, this is a form of load balancing.
    - Another way that reverse proxies are commonly used by popular websites is to deal with decryption
    - Reverse proxies are now implemented in order to use hardware built specifically for cryptography, to perform the enryption and decryption work. So that the web servers are free to just serve content.

---

## Connecting to the Internet

A dial-up connection uses POTS (Plain old telephone system, aka PSTN public switched Telephone n/w) for data transfer, and gets its name because the connection is established by actually dialing a phone number

A baud rate is a measurement of the number of bits that can be sent across a telephone every second.

### BroadBand

The term broadband has a few definitions.

In terms of internet connectivity, it's used to refer to any connectivity technology that isn't dial-up Internet.

Broadband Internet is almost always much faster than even the fastest dial-up connections and refers to connections that are always on.

Without broadband internet connection technologies, the Internet as we know it today wouldn't exist.

- A single picture taken on a smartphone today can easily be several megabytes in size.
- Two megabytes would translate to 16,777,216 bits.
- At a baud rate of 14.4 kilobits per second, that many bits would take nearly 20 minutes to download.

### T-Carrier Technologies

- T-Carrier Technologies were first invented by AT&T in order to provision a system that allowed lots of phone calls to travel across a single cable.
- Every individual phone call was made over individual pairs of copper wire before Transmission System 1, the first T-Carrier specification called T1 for short.
- With the T1 specification, AT&T invented a way to carry up to 24 simultaneous phone calls across a single piece of twisted pair copper.
- Years later, the same technology was re-purposed for data transfers.
- Each of the 24 phone channels was capable of transmitting data at 64 kilobits per second, making a single T1 line capable of transmitting data at 1.544 megabits per second.
- Over the years, the phrase T1 has come to mean any twisted-pair copper connection capable of speeds of 1.544 megabits per second even, if it doesn't strictly follow the original Transmission System 1 specification
- Originally, T1 technology was only used to connect different telecom company sites to each other and to connect these companies to other telecom companies. But with the rise of the internet as a useful business tool in the 1990s, more and more businesses started to pay to have T1 lines
  installed at their offices to have faster internet connectivity.
- More improvements to the T1 line were made by developing a way of multiple T1s to act as a single link.
- So, T3 line is 28 T1s all multiplexed achieving a total throughput speed of 44.736 megabits per second.

### Digital Subscriber Link (DSL)

- Twisted pair copper used by modern telephone lines was capable of transmitting way more data than what was needed for voice-to-voice calls.
- By operating at a frequency range that didn't interfere with normal phone calls, a technology known as digital subscriber line or
  DSL was able to send much more data across the wire than traditional dial-up technologies.
- To top it all off, this allowed for normal voice phone calls and data transfer to occur at the same time on the same line.
- DSL technologies also use their own modems. They're known as DSLAMs or Digital Subscriber Line Access Multiplexers.
  Just like dial-up modems, these devices establish data connections across phone lines, but unlike dial-up connections, they're usually long-running.
  This means that the connection is generally established when the DSLAM is powered on and isn't torn down until the DSLAM is powered off.
- the two most common types of DSL are ADSL and SDSL.
  - **ADSL** stands for Asymmetric Digital Subscriber Line.
  - ADSL connections feature different speeds for outbound and incoming data.
  - Generally, this means faster download speeds and slower upload speeds
  - **SDSL** stands for Symmetric Digital Subscriber Line.
  - SDSL technology is basically the same as ADSL except the upload and download speeds are the same.

### Cable Broadband

- Much like how DSL was developed, cable companies quickly realized that the coaxial cables generally used by cable television delivery into a person's
  home were capable of transmitting much more data than what was required for TV viewing.

- By using frequencies that don't interfere with television broadcast, cable-based Internet access technologies were able to deliver high speed Internet access across these same cables. This is the technology that we refer to when we say cable broadband.

- These are Shared bandwidth broadband connections

### Fiber Connections

- In recent years, it's become popular to use fiber to deliver data closer and closer to the end user.
- Exactly how close to the end user can vary a ton across implementations, which is why the phrase FTTX was developed.
- FTTX stands for fiber to the X, where the X can be one of many things.
- FTTN, which means fiber to the neighborhood.
- FTTB is a setup where fiber technologies are used for data delivery to an individual building
- FTTH, which stands for fiber to the home.
- FTTH and FTTB may both also be referred to as FTTP, fiber to the premises.
- Instead of a modem, the demarcation point for fiber technologies is known as Optical Network Terminator, or ONT.
- An ONT converts data from protocols the fiber network can understand to those that are more traditional twisted pair copper networks can understand.

#### Protocols related to Fibers

In computer networking, Point-to-Point Protocol (PPP) is a data link layer (layer 2) communications protocol between two routers directly without any host or any other networking in between. It can provide connection authentication, transmission encryption, and compression.

Two derivatives of PPP, Point-to-Point Protocol over Ethernet (PPPoE) and Point-to-Point Protocol over ATM (PPPoA), are used most commonly by ISPs to establish a digital subscriber line (DSL) Internet service connection with customers.

### Wide Area Networks

- A wide area network acts like a single network but spans across multiple physical locations.
  WAN technologies usually require that you contract a link across the Internet with your ISP.
- This ISP handles sending your data from one side to the other. So, it could be like all of your computers are in the same physical location.
- In a WAN, the area between a demarcation point (the point where the n/w ends) and the ISP's core network is known as a local loop

### Point-to-Point VPNs

A popular alternative to WAN technologies are point-to-point VPNs.

A point-to-point VPN, also called a site-to-site VPN, establishes a VPN tunnel between two sites.

This operates a lot like the way that a traditional VPN setup lets individual users act as if they are on the network they're connecting to.

It's just that the VPN tunneling logic is handled by network devices at either side, so that users don't all have to establish their own connections

# Computer Networks

## High-level view of the internet

The Internet is a computer network that interconnects billions of computing devices throughout the world.

In Internet jargon, all of these devices are called hosts or end systems.

End systems are connected together by a network of communication links and packet switches

there are many types of communica-tion links, which are made up of different types of physical media, including coaxial cable, copper wire, optical fiber, and radio spectrum

Different links can transmit data at different rates, with the transmission rate of a link measured in bits/second.

When one end system has data to send to another end system, the sending end system segments the data and adds header bytes to each segment. The resulting packages of information, known as packets in the jargon of computer networks, are then sent through the network to the destination end system, where they are reassembled into the original data.

A packet switch takes a packet arriving on one of its incoming communication links and forwards that packet on one of its outgoing communication links. Packet switches come in many shapes and flavors, but the two most prominent types in today’s Internet are routers and link-layer switches.

Link-layer switches are typically used in access networks, while routers are typically used in the network core. The sequence of communication links and packet switches traversed by a packet from the sending end system to the receiving end system is known as a route or path.

packets are analogous to trucks, communication links are analogous to highways and roads, packet switches are analogous to intersections, and end systems are analogous to buildings. Just as a truck takes a path through the transportation network, a packet takes a path through a computer network.

End systems, packet switches, and other pieces of the Internet run protocols that control the sending and receiving of information within the Internet. The Transmission Control Protocol (TCP) and the Internet Protocol (IP) are two of the most impor-tant protocols in the Internet. The IP protocol specifies the format of the packets that are sent and received among routers and end systems. The Internet’s principal protocols are collectively known as TCP/IP.

End systems attached to the Internet provide a **socket interface** that specifies how a program running on one end system asks the Internet infrastructure to deliver data to a specific destination program running on another end system. This Internet socket interface is a set of rules that the sending program must follow so that the Internet can deliver the data to the destination program.

Suppose Alice wants to send a letter to Bob using the postal service. Alice, of course, can’t just write the letter (the data) and drop the letter out her window. Instead, the postal service requires that Alice put the letter in an envelope; write Bob’s full name, address, and zip code in the center of the envelope; seal the envelope; put a stamp in the upper-right-hand corner of the enve-lope; and finally, drop the envelope into an official postal service mailbox. Thus, the postal service has its own “postal service interface,” or set of rules, that Alice must follow to have the postal service deliver her letter to Bob. In a similar manner, the Internet has a socket interface that the program sending data must follow to have the Internet deliver the data to the program that will receive the data.

### Internet Protocols

- A protocol defines the format and the order of messages exchanged between two or more communicating entities, as well as the actions taken on the transmission and/or receipt of a message or other event.

- The Internet, and computer networks in general, make extensive use of pro-tocols. Different protocols are used to accomplish different communication tasks.

### End Systems or hosts (the network edge)

- The Internet’s end systems include desktop computers (e.g., desktop PCs, Macs, and Linux boxes), servers (e.g., Web and e-mail servers), and mobile devices (e.g., laptops, smartphones, and tablets). Furthermore, an increasing number of non-traditional “things” are being attached to the Internet as end systems.

- End systems are also referred to as hosts because they host (that is, run) application programs such as a Web browser program, a Web server program, an e-mail client program, or an e-mail server program.

- Hosts are sometimes further divided into two categories: clients and servers. Informally, cli-ents tend to be desktop and mobile PCs, smartphones, and so on, whereas serv-ers tend to be more powerful machines that store and distribute Web pages, stream video, relay e-mail, and so on. Today, most of the servers from which we receive search results, e-mail, Web pages, and videos reside in large data centers.

### Access Networks

- the network that physically connects an end system to the first router (also known as the “edge router”) on a path from the end system to any other distant end system

### Packet Switching

To send a message from a source end system to a destination end system, the source breaks long messages into smaller chunks of data known as packets. Between source and destination, each packet travels through communication links and packet switches (for which there are two predominant types, routers and link-layer switches). Packets are transmitted over each communication link at a rate
equal to the full transmission rate of the link. So, if a source end system or a packet
switch is sending a packet of L bits over a link with transmission rate R bits/sec, then
the time to transmit the packet is L / R seconds.

### Store-and-Forward Transmission

Most packet switches use store-and-forward transmission at the inputs to the
links. Store-and-forward transmission means that the packet switch must receive
the entire packet before it can begin to transmit the first bit of the packet onto the
outbound link

Let’s now consider the general case of sending one packet from source to destination over a path consisting of N links each of rate R (thus, there are N-1 routers
between source and destination). Applying the same logic as above, we see that the
end-to-end delay is:
delay = N \* L / R

### Queuing Delays and Packet Loss

Each packet switch has multiple links attached to it. For each attached link, the
packet switch has an output buffer (also called an output queue), which stores
packets that the router is about to send into that link. The output buffers play a key
role in packet switching. If an arriving packet needs to be transmitted onto a link but
finds the link busy with the transmission of another packet, the arriving packet must
wait in the output buffer. Thus, in addition to the store-and-forward delays, packets
suffer output buffer queuing delays. These delays are variable and depend on the
level of congestion in the network. Since the amount of buffer space is finite, an arriving packet may find that the buffer is completely full with other packets waiting
for transmission. In this case, packet loss will occur—either the arriving packet or
one of the already-queued packets will be dropped.

### Forwarding Tables and Routing Protocols

In the Internet, every end system has an address called an IP address. When a
source end system wants to send a packet to a destination end system, the source
includes the destination’s IP address in the packet’s header. As with postal addresses,
this address has a hierarchical structure. When a packet arrives at a router in the net-
work, the router examines a portion of the packet’s destination address and forwards
the packet to an adjacent router. More specifically, each router has a forwarding
table that maps destination addresses (or portions of the destination addresses) to that
router’s outbound links. When a packet arrives at a router, the router examines the
address and searches its forwarding table, using this destination address, to find the
appropriate outbound link. The router then directs the packet to this outbound link.

The Internet has a number of special routing protocols that are used to auto-
matically set the forwarding tables. A routing protocol may, for example, determine
the shortest path from each router to each destination and use the shortest path results
to configure the forwarding tables in the routers.

### Circuit Switching

In circuit-switched networks, the resources needed along a path (buffers, link
transmission rate) to provide for communication between the end systems are
reserved for the duration of the communication session between the end systems.

In packet-switched networks, these resources are not reserved; a session’s messages
use the resources on demand and, as a consequence, may have to wait (that is, queue)
for access to a communication link.

Traditional telephone networks are examples of circuit-switched networks.

---

## The 4-layer Internet Model

Application -> Transport -> Network -> Link

### Datagrams

- Datagrams is a packet with content and header, i.e. `| Data | From | To |`

### Link

- The Internet is made up of end-hosts, links and routers. Data is delivered hop-by-hop over each link in turn. Data is delivered in packets. A packet consists of the data we want to be delivered, along with a header that tells the network where the packet is to be delivered, where it came from and so on.

- The Link Layer’s job is to carry the data over one link at a time. Ethernet and WiFi are two examples of different Link layers.

### Network

- Network layer is special because

  - IP makes a best-effort attempt to deliver our packets to the other end. But it makes no promises.
  - IP packets can get lost, can be delivered out of order, and can be corrupted. There are no guarantees.

- How can the Internet work at all when the packets are not guaranteed to be delivered? Well, if an application wants a guarantee that its data will be re-transmitted when necessary and will be delivered to the application in order and without corruption then it needs another protocol running on top of IP. This is the job of the Transport Layer.

### Transport

- The most common Transport layer is the TCP (Transmission Control Protocol).
- TCP/IP is when an application uses both TCP and IP together
- TCP makes sure that data sent by an application at one end of the Internet is correctly delivered – in the right order - to the application at the other end of the Internet
- If the Network Layers drops some datagrams, TCP will retransmit them, multiple times if need-be
- If the Network Layer delivers them out of order – perhaps because two packets follow a different path to their destination – TCP will put the data back into the right order again
- The main thing to remember is that TCP provides a service to an application guaranteeing correct in-order delivery of data, running on top of the Network Layer service, which provides an unreliable datagram delivery service.
- Applications such as a web client, or an email client, find TCP very useful indeed. By employing TCP to make sure data is delivered correctly, they don’t have to worry about implementing all of the mechanisms inside the application. They can take advantage of the huge effort that developers put into correctly implementing TCP, and reuse it to deliver data correctly. Reuse is another big advantage of layering.
- UDP (User Datagram Protocol) is an alternative communications protocol to Transmission Control Protocol (TCP) used primarily for establishing low-latency and loss-tolerating connections between applications on the internet.

### Application

- There are many thousands of applications that use the Internet. While each application is different, it can reuse the Transport Layer by using the well-defined API from the Application Layer to the TCP or UDP service beneath.
- Applications typically want a bi-directional reliable byte stream between two end points. They can send whatever byte-stream they want, and Applications have a protocol of their own that defines the syntax and semantics of data flowing between the two end points.
- As far as the Application Layer is concerned, the GET request is sent directly to its peer at the other end – the web server Application. The Application doesn’t need to know how it got there, or how many times it needed to be retransmitted. At the web client, the Application Layer hands the GET request to the TCP layer, which provides the service of making sure it is reliably delivered. It does this using the services of the Network layer, which in turn uses the services of the Link Layer.

We say that each layer communicates with its peer layer. It’s as if each layer is only communicating with the same layer at the other end of the link or Internet, without regard for how the layer below gets the data there.

<details>
  <summary> Summary </summary>

![4 Layer model summary](./../../images/4-layer.png)

</details>

- Two further things to note

  - The first is that IP is often referred to as “the thin waist” of the Internet. This is because if we want to use the Internet, we have to use the Internet Protocol, or IP. We have no choice.
    - But we have lots of choices for Link Layers: IP runs over many different Link Layers, such as Ethernet, WiFi, DSL, 3G cellular, and so on.
    - On top of the unreliable IP layer, we can choose between many different transport layers. We already saw TCP and UDP. There is RTP for real time data and many others too. And of course there are tens of thousands of different applications
  - The second is that in the 1980s the International Standards Organization, or ISO created a 7-layer model to represent any type of network. It was called the 7-layer Open Systems Interconnection or OSI model.
    - it has been replaced by the 4-layer Internet model.
    - Today, the only real legacy of the 7-layer OSI model is the numbering system. You’ll often hear network engineers refer to the Network Layer as “Layer 3”, even though it is the 2nd layer up from the bottom in the Internet Layer. Similarly, you’ll hear people refer to Ethernet as a Layer 2 protocol, and the Application as Layer 7.

  <details>

  <summary>7 Layer model</summary>

  ![7 layer model](../../images/7-layer.png)

  </details>

  ***

## 7 layer model (OSI Model - Open Systems Interconnection) (OSI)

- It's a conceptual framework. The model uses layers to help give a visual description of what is going on with a particular networking system.
- It has been developed by ISO – ‘International Organization of Standardization‘, in the year 1984. It is a 7 layer architecture with each layer having specific functionality to perform.

<details>

<summary>7 Layer model</summary>

![OSI Model](../../images/OSI-model.png)

</details>

#### Layer 7 - Application

- In the OSI model, this is the layer that is the “closest to the end user”. Applications that work at Layer 7 are the ones that users interact with directly. A web browser (Google Chrome, Firefox, Safari, etc.) or other app - Skype, Outlook, Office - are examples of Layer 7 applications.

#### Layer 6 - Presentation

- Presentation layer is also called the Translation layer.The data from the application layer is extracted here and manipulated as per the required format to transmit over the network.
- The functions of the presentation layer are :
  - **Translation** : For example, ASCII to EBCDIC.
  - **Encryption/ Decryption** : Data encryption translates the data into another form or code. The encrypted data is known as the cipher text and the decrypted data is known as plain text. A key value is used for encrypting as well as decrypting data.
  - **Compression**: Reduces the number of bits that need to be transmitted on the network.

#### Layer 5 - Session

- This layer is responsible for establishment of connection, maintenance of sessions, authentication and also ensures security.
  The functions of the session layer are :
  - **Session establishment, maintenance and termination**: The layer allows the two processes to establish, use and terminate a connection.
  - **Synchronization** : This layer allows a process to add checkpoints which are considered as synchronization points into the data. These synchronization point help to identify the error so that the data is re-synchronized properly, and ends of the messages are not cut prematurely and data loss is avoided.
  - **Dialog Controller** : The session layer allows two systems to start communication with each other in half-duplex or full-duplex.

_All the above 3 layers(including Session Layer) are integrated as a single layer in the TCP/IP model as “Application Layer”_

#### Layer 4 - Transport

- Transport layer provides services to application layer and takes services from network layer. The data in the transport layer is referred to as Segments. It is responsible for the End to End Delivery of the complete message. The transport layer also provides the acknowledgement of the successful data transmission and re-transmits the data if an error is found.

#### Layer 3 - Network

Network layer works for the transmission of data from one host to the other located in different networks. It also takes care of packet routing i.e. selection of the shortest path to transmit the packet, from the number of routes available. The sender & receiver’s IP address are placed in the header by the network layer.
The functions of the Network layer are :

- **Routing**: The network layer protocols determine which route is suitable from source to destination. This function of network layer is known as routing.
- **Logical Addressing**: In order to identify each device on internetwork uniquely, network layer defines an addressing scheme. The sender & receiver’s IP address are placed in the header by network layer. Such an address distinguishes each device uniquely and universally.

#### Layer 2 - Data Link Layer

The data link layer is responsible for the node to node delivery of the message. The main function of this layer is to make sure data transfer is error-free from one node to another, over the physical layer. When a packet arrives in a network, it is the responsibility of DLL to transmit it to the Host using its MAC address.

#### Layer 1 - Physical

- The lowest layer of the OSI reference model is the physical layer. It is responsible for the actual physical connection between the devices. The physical layer contains information in the form of bits. It is responsible for transmitting individual bits from one node to the next. When receiving data, this layer will get the signal received and convert it into 0s and 1s and send them to the Data Link layer, which will put the frame back together.

## The Internet Protocol

<details>
  <summary>The Internet Protocol (diagram)</summary>

![IP](../../images/ipservice.png)

</details>

<details>
  <summary>IP Service model (diagram)</summary>

![IP](../../images/ipservicemodel.png)

</details>

1. Datagrams
   - IP is a datagram service. When we ask IP to send some data for us, it creates a datagram and puts our data inside. The datagram is a packet that is routed individually through the network based on the information in its header. In other words, the datagram is self-contained.
   - The header contains the IP address of the destination, which we abbreviate here as “IP DA” for IP destination address. The forwarding decision at each router is based on the IP DA.
   - The datagram header also contains an IP source address, or “IP SA”, saying where the packet came from, so the receiver knows where to send any response.
   - Datagrams are routed hop-by-hop through the network from one router to the next, all the way from the IP source address to the IP destination address.
2. Unreliable
   - IP is unreliable. IP makes no promise that packets will be delivered to the destination. They could be delivered late, out of sequence, or never delivered at all. It’s possible that a packet will be duplicated along the way, for example by a misbehaving router.
3. Best Efforts
   - IP does make the promise to only drop datagrams if necessary. For example, the packet queue in a router might fill up because of congestion, forcing the router to drop the next arriving packet. IP won’t make any attempt to resend the data – in fact, IP doesn’t tell the source that the packet was dropped.
4. Connectionless
   - IP is an extremely simple, minimal service. It maintains no state at all related to a communication. We say that a communication service is “connectionless”, because it doesn’t start by establishing some end to state associated with the communication.

### Why is the IP service so simple?

- After all, it is the foundation of the entire Internet. Every communication over the Internet uses – must use – the IP service. Given how important the Internet is, wouldn’t it have been better to make IP reliable? After all, we did say that most applications want a reliable, byte-communication service.

Reasons

1. To keep the network simple, dumb and minimal. Faster, more streamlined and lower cost to build and maintain.
2. **The end to end principle**: Where possible, implement features in the end hosts.
3. Allows a variety of reliable (or unreliable) services to be built on top. If IP was reliable – in other words if any missing packets were retransmitted automatically – then it would not be ideal for some services. For example, in real time applications like a video chat, there might be no point in retransmitting lost data, because it might arrive too late to be useful. Instead, the application might choose to show a few blank pixels or use the pixels from the frame before. By not providing any reliability guarantees, IP lets the application choose the reliability service its needs.
4. Works over any link layer: IP makes very few assumptions about the link layer. IP makes very little expectation of the Link layer below – the link could be wired or wireless, and requires no retransmission or control of congestion

### Other IP Services

1. IP tries to prevent packets from looping forever. Because IP routers forward packets hop-by-hop across the Internet, it is possible for the forwarding table in a router to be wrong, causing a packet to start looping round and around following the same path. This is most likely to happen when the forwarding tables are changing and they temporarily get into an inconsistent state. Rather than try to prevent loops from ever happening – which would take a lot of complexity - IP uses a very simple mechanism to catch and then delete packets that appear to be stuck in a loop. To do this, IP simply adds a **hop-count** field in the header of every datagram. It is called the **time to live, or TTL field**. It starts out at a number like 128 and then is decremented by every router it passes through. If it reaches zero, IP concludes that it must be stuck in a loop and the router drops the datagram. It is a simple mechanism, typical of IP – it doesn’t guarantee loops won’t happen, it just tries to limit the damage caused by a flood of endlessly looping packets in the network.
2. IP will fragment packets if they are too long. IP is designed to run over any kind of link. Most links have a limit on the size of the packets they can carry. For example, Ethernet can only carry packets shorter than 1500bytes long. If an application has more than 1500bytes to send, it has to be broken into 1500 pieces before sending in an IP datagram. Now, along the path towards the destination, a 1500byte datagram might need to go over a link that can only carry smaller packets, for example 1000 bytes. The router connecting the two links will fragment the datagram into two smaller datagrams.
3. IP uses a header checksum to reduce chances of delivering a datagram to the wrong destination. IP includes a checksum field in the datagram header to try and make sure datagrams are delivered to the right location. It could be quite a security problem if packets are accidentally and frequently sent to the wrong place because of a mistake by a router along the way.
4. There are two versions of IP in use today: IPv4, which is used today by over 90% of end hosts. It uses the 32bit addresses you are probably familiar with. Because we are running out of IPv4 addresses, the Internet is in a gradual transition to IPv6, which uses 128 bit addresses instead.
5. Finally, IP allows new fields to be added to the datagram header. This is a mixed blessing. On the one hand, it allows new features to be added to the header that turn out to be important, but weren’t in the original standard. On the other hand, these fields need processing and so require extra features in the routers along the path, breaking the goal of a simple, dumb, minimal forwarding path. In practice, very few options are used or processed by the routers.

### IPv4 Diagram

<details>
  <summary>IPv4 address Diagram</summary>

![IPv4](../../images/ipv4.png)

</details>

1. Above is a picture of an IPv4 header, which is the most common header in use today. It's drawn here in 32 bit words, with “Bit 0” the first to be sent onto the wire.
2. The Protocol ID, that tells us what is inside the data field. Essentially, it allows the destination end host to demultiplex arriving packets, sending them to the correct code to process the packet. If the Protocol ID has the value “6” then it tells us the data contains a TCP Segment and so we can safely pass the datagram to the TCP code and it will be able to parse the segment correctly. The Internet Assigned Numbers Authority (IANA) defines over 140 different values of Protocol ID, representing different transport protocols.
3. The Version tells us which version of IP – currently, the legal values are IPv4 and IPv6. This header is an IPv4 header
4. The Total packet length can be up 64kBytes including the header and all the data.
5. The “Time to Live” field helps us to prevent packets accidentally looping in the network forever. Every router is required to decrement the TTL field. If it reaches zero, the router should drop the packet.
6. Sometimes a packet is too long for the link it is about to be sent on. The Packet ID, Flags and Fragment Offset all help routers to fragment IP packets into smaller self-contained packets if need-be.
7. The Type of Service field gives a hint to routers about how important this packet is.
8. The Header Length tells us how big the header is --- some headers have optional extra fields to carry extra information.
9. Finally, a checksum is calculated over the whole header so just in case the header is corrupted, we are not likely to deliver a packet to the wrong destination by mistake.

---

## Life of a packet

- Almost all web traffic is over TCP, the Transport Control Protocol. In its typical operation, there’s a client and a server. A server listens for connection requests. To open a connection, a client issues a connection request, which the server responds to.
- This exchange takes three messages, something called the “three way handshake.”

### SYN, SYN-ACK, SYN

- The first step of handshake is when the client sends a “synchronize” message to the server, often called a **SYN**. The second step is when the server responds with a “synchronize” message that also acknowledges the clients “synchronize”, or a “synchronize and acknowledge message”, often called a **SYN-ACK**. The third and final step is when the client responds by acknowledging the server’s synchronize, often called an **ACK**. So often the three way handshake is described as “synchronize, synchronize and acknowledge, acknowledge”, or **“SYN, SYN-ACK, ACK”**.

### TCP Byte Stream

<details>
  <summary>Diagram: </summary>

![TCP Byte Stream](../../images/tcpbytestream.png)

</details>

- **Recall that the network layer is responsible for delivering packets to computers, but the transport layer is responsible for delivering data to applications.**
- From the perspective of the network layer, packets sent to different applications on the same computer look the same. This means that to open a TCP stream to another program, we need two addresses.
- The first, an Internet Protocol address (IP Address), is the address the network layer uses to deliver packets to the computer.
- The second, the TCP port, tells the computer’s software which application to deliver data to.
- Web servers usually run on TCP port 80. So when we open a connection to a web server, we send IP packets to the computer running the web server whose destination address is that computer’s IP address. Those IP packets have TCP segments whose destination port is 80.

#### Hopping

- But how do those IP packets get to their destination? We don’t have a direct wire connecting my client to the server.
- Instead, the client is connected to an intermediate computer, a router. This router is itself connected to other routers.
- IP packets between the client and server take many “hops,” where a hop is a link connecting two routers. For example, since my client is on a WiFi network, the first hop is wireless to the WiFi access point. The access point has a wired connection to the broader Internet, so it forwards my client’s packets along this wired hop.
- A router can have many links connecting it. As each packet arrives, a router decides which of its links to send it out on. Routers have IP addresses, so it’s also the case that it might not forward a packet but rather deliver it to its own software. For example, when you log into a router using TCP, the IP packets are destined to the router’s own IP address.

### Inside a Router: Forwarding Tables

<details>
  <summary>Diagram: </summary>

![Forwading Tables](../../images/insideHop.png)

</details>

- How does a router make this decision? It does so through something called a **forwarding table**, shown here on the right. A forwarding table consists of a set of IP address patterns and the link to send across for each pattern.
- When a packet arrives, the router checks which forwarding table entry’s pattern best matches the packet. It forwards the packet along that entry’s link. Generally, “best” means the most specific match.
- The default route is the least specific route -- it matches every IP address. If, when a packet arrives, there isn’t a more specific route than the default route, the router will just use the default one.
- The default route is especially useful in edge networks. For example, Stanford University has a router connecting it to the larger Internet. The router will have many specific routes for the IP addresses of Stanford’s network. But if the destination IP address isn’t Stanford’s, then the router should send it out to the larger Internet.

---

## Packet Switching basics

- Packet: A self-contained unit of data that carries information necessary for it to reach its destination.
- Packet switching: Independently for each arriving packet, pick its outgoing link. If the link is free, send it. Else hold the packet for later.

### Two consequences

- Simple Packet forwarding
- Efficient sharing of links

### No per-flow state required

- Flow: A collection of datagrams belonging to the same end-to-end communication, e.g. a TCP connection.

- Packet switches don’t need state for each flow – each packet is self-contained.:

- No per-flow state to be added/removed.:

  - The switch doesn’t need to worry about adding or removing this per-flow state. Imagine if every time you wanted to load a web page, you had to communicate with every switch along the path to set up state so your request would work. This could make things much slower. Instead, you can just send packets and the switches forward them appropriately.

- No per-flow state to be stored.

  - The switches also don’t need to _store_ this state. Because switches have to be fast, they’d need to store this state in very fast memory, which is expensive. This lets switches focus on doing one thing, forwarding packets quickly.

- No per-flow state to be changed upon failure.

### Efficient sharing of links

Data traffic is bursty

- Packet switching allows flows to use all available link capacity.
- Packet switching allows flows to share link capacity.

This is called Statistical Multiplexing.

- This idea of taking a single resource and sharing it across multiple users in a probabilistic or statistical way is called statistical multiplexing. It’s statistical in that each user receives a statistical share of the resource based on how much others are using it. For example, if your friend is reading, you can use all of the link. If both of you are loading a page, you receive half of the link capacity.

#### Quiz

- In the traditional circuit-switched telephone network, the human voice is digitized at 64kb/s. The following reasons explain why circuit switching was a good choice for the telephone network? Check all that apply.
  - Multiplexing many 64kb/s channels onto an electrical, optical or wireless link is quite easy. The fixed rate of each circuit makes it easy to manage the link.
  - By dedicating 64kb/s to every phone call, the quality can be kept constant without fear of one phone call interfering with the quality of another.

---

## Layering

- Layers are functional components
- Layers communicate sequentially with the layer above and below.
- Each layer provides a well-defined service to the layer above, using the services provided by layers below and its own private processing.

Example of layering

- Programming: Source code -> byte code -> assembly code -> machine code
- Postal service: person1 -> mailbox -> local mail service -> transport -> local mail service 2 -> mailbox -> user 2

Reasons for layering:

1. Modularity
2. Well defined service
3. Reuse
4. Separation of concerns
5. Continuous improvement
6. Peer-to-peer communications (TCP communicating only with TCP)

---

## Encapsulation

Encapsulation is how layering manifests in data representation.

- Encapsulation is the result of what happens when you combine layers and packet switching.
- We want to break up our data into discrete units, called packets. However, each packet contains data from multiple layers. When you send a TCP segment, for example, it’s inside an IP packet, which is inside an Ethernet frame. Encapsulation is how this works. Encapsulation is the principle by which you organize information in packets so that you can maintain layers, yet let them share the contents of your packets.
- Encapsulation allows each layer to grow and evolve independently
- Examples:
  - HTTP (web) payload inside
  - a TCP Transport Segment
  - an IP network packet
  - a WiFi Link frame
- Encapsulation allows us to layer recursively.
  - Example: VPN
    - HTTP (web) application payload in
    - a TCP transport segment in
    - an IP network packet in
    - a secured TLS presentation message in
    - a TCP transport segment in
    - an IP network packet in
    - an Ethernet link frame.

---

## Endianness

- How a multi byte value is laid out in memory is called endianness.
- Little Endian - Least Significant Digit is at lower addresss
- Big Endian - MSB is at lowest address

e.g. 1024 Decimal = 0x0400 Binary (in Bytes) (means 16 bits)

so

- 00 | 04 would be Little endian
- 04 | 00 would be Big endian

- Tip to remember (Little Endian is the reverse order, big endian is in normal writing form)

- Why does this matter? If two computers need to communicate, they need to agree how to represent multi-byte fields.
- DIfferent processors have different endianness e.g. x86 is little endian vs ARM is big endian
- You have to convert network byte order values to your host order.
- Network byte order is Big-endian by convention

- The layout of a character string (8-bit ASCII) is the same on big-endian and little endian architentures. This is true coz:

  If you have a simple 8-bit character representation (e.g. extended ASCII), then no, endianness does not affect the layout, because each character is one byte.

  If you have a multi-byte representation, such as UTF-16, then yes, endianness is still important

---

## Names and Addresses: IPv4

- an IPv4 Address identifies a device on the internet (Layer 3 network address)
- 32 bits long written in the format `a.b.c.d`
  - 171.64.64.64 (In decimal)
  - 192.168.123.132 (Decimal), 110000000101000111101110000100 (binary), 11000000.10101000.01111011.10000100 (binary octets)
- For a TCP/IP wide area network (WAN) to work efficiently as a collection of networks, the routers that pass packets of data between networks do not know the exact location of a host for which a packet of information is destined. Routers only know what network the host is a member of and use information stored in their route table to determine how to get the packet to the destination host's network. After the packet is delivered to the destination's network, the packet is delivered to the appropriate host.
- For this process to work, an IP address has two parts. The first part of an IP address is used as a network address, the last part as a host address.
- The second item, which is required for TCP/IP to work, is the subnet mask. The subnet mask is used by the TCP/IP protocol to determine whether a host is on the local subnet or on a remote network.
- In TCP/IP, the parts of the IP address that are used as the network and host addresses are not fixed, so the network and host addresses above cannot be determined unless you have more information. This information is supplied in another 32-bit number called a subnet mask

Let's say that the subnet mask is `255.255.255.0`. It is not obvious what this number means unless you know that `255` in binary notation equals `11111111`; so, the subnet mask is:

`11111111.11111111.11111111.0000000`

Lining up the IP address and the subnet mask together, the network and host portions of the address can be separated:

`11000000.10101000.01111011.10000100 -- IP address (192.168.123.132)`
`11111111.11111111.11111111.00000000 -- Subnet mask (255.255.255.0)`

The first 24 bits (the number of ones in the subnet mask) are identified as the network address, with the last 8 bits (the number of remaining zeros in the subnet mask) identified as the host address. This gives you the following:

`11000000.10101000.01111011.00000000 -- Network address (192.168.123.0)`
`00000000.00000000.00000000.10000100 -- Host address (000.000.000.132)`

So now you know, for this example using a `255.255.255.0` subnet mask, that the network ID is `192.168.123.0`, and the host address is `0.0.0.132`. When a packet arrives on the `192.168.123.0` subnet (from the local subnet or a remote network), and it has a destination address of `192.168.123.132`, your computer will receive it from the network and process it.

### Address Structure

Originally, 3 classes of addresses

- Class A: | 0 | Network (7) | host (24) |
- Class B: | 1 | 0 | Network (14) | host(16) |
- Class C: | 1 | 1 | 1 | Network (21) | host(8) |

Now: Classless Inter-Domain Routing (CIDR)

- Today, IPv4 addresses are structured thought something called CIDR, or Classless Inter-Domain Routing. Rather than have prefixes only of length 8, 16, and 24 bits, CIDR allows prefixes to be any number of bits. This means all CIDR prefixes define a block of addresses that is a power of 2 in size. When we talk about a CIDR address, we refer to its netmask length. So, for example, when we talk about a “slash 16”, we mean a netmask of length 16. This CIDR block describes 2 to the 16 addresses, or 65,536. When we talk about a “slash 20”, we mean a netmask of length 20. This CIDR block describes 2 to the 12 addresses, or 4.096 addresses. CIDR blocks are how addresses are structured, addressed, and managed today.

- Address block is a pair: address and count
- counts are power of 2, which specify netmask length
- 171.64.0.0/16 means any address in the range 171.64.0.0 to 171.64.255.255
- A /24 describes 256 addresses, a /20 describes 4096 addresses

### IPv4 Address Assignment

- IANA: Internet Assigned Numbers Authority
  - Internet Corporation for Assignment of Names and Numbers (ICANN)’s job
- IANA gives out /8s to Regional Internet Registries (RIRs)
  - Ran out in February 2011, in special end case of giving 1 to each RIR
- RIRs responsible for geographic regions, each has own policy
  - AfriNIC: Africa
  - ARIN: U.S.A., Canada, Carribean, Antarctica
  - APNIC: Asia, Australia, New Zealand
  - LACNIC: Latin America, Carribean
  - RIPE NCC: Europe, Russia, Middle East, Central Asia

There’s an organization called IANA, for the Internet Assigned Numbers Authority. The ultimate authority is ICANN, the Internet Corporation for Assignment of Names and Numbers. ICANN delegates the work to IANA.

IANA gives out slash-8s, describing 16 million addresses, to Regional Internet Registries or RIRs. Each continent has its own RIR. The RIR for the United States is ARIN, while the RIR for the western Pacific is APNIC. These RIRs each have their own policy for how they break up the /8s into smaller blocks of addresses and assign them to parties who need them.You might have read in the news is that we’ve run out of IP addresses. This isn’t really true -- there are many unused addresses today. What _did_ happen is that IANA ran out of /8s to give out. It reached a special end case in its charter. When reduced to its last 5 /8s, IANA gave one /8 to each RIR. Now address management and allocation is up to RIRs

## Longest Prefix Match

- Internet routers can have many links. They have many options for which direction to forward a received packet. To select which link to forward a packet over, routers today typically use an algorithm called Longest Prefix Match.
- When a packet arrives, the router checks which forwarding table entry best matches the packet and forwards the packet along the link associated with that forwarding table entry. By “best”, I mean most specific. The default route is effectively all wildcards -- it matches every IP address. If, when a packet arrives, there isn’t a more specific route than the default route, the router will just use the default one.
- Hence - **LPM** - is an Algorithm IP routers use to chose matching entry from forwarding table

Quiz

 <details>
  <summary>Diagram: </summary>

![LMP Quiz](../../images/Lpm-quiz.png)

</details>

<details>
  <summary>Answers: </summary>

- The answer for A, 63.19.5.3, is link 3. 63.19.5.3 matches two prefixes: the default route and prefix 63.19.5.0/30. The prefix is 30 bits long and 63.19.5.3 differs in only the last two bits. /30 is a longer prefix than /0 so the router will pick link 3.
- The answer for B,171.15.15.0, is link 4. 171.15.15.0 matches three entries. It matches the default route, 171.0.0.0/8 and 171.0.0.0/10. It does not match 171.0.15.0/24 because B’s second octet is 15, not 0. The third match, 171.0.0.0/10, is the longest prefix, so the router sends the packet along link 4.
- The answer for C, 63.19.5.32, is link 1. The longest prefix match is the default route. It does not match 63.19.5.0/30 because it differs in the 26th bit.
- The answer for D, 44.199.230.1, is link 1. The longest prefix match is the default route.
- The answer for E, 171.128.16.0, is link 2. This address matches two prefixes, the default route and 171.0.0.0/8. It does not match 171.0.0.0/10 because it differs on the 9th bit. 171.0.0.0/8 is the longest prefix, so the router will forward the packet on link 2.
  </details>

---

## Address Resolution Protocol (ARP)

- The Address Resolution Protocol, or ARP, is the mechanism by which the network layer can discover the link address associated with a network address it’s directly connected to.
- Put another way, it’s how a device gets an answer to the question: “I have an IP packet whose next hop is this address -- what link address should I send it to?”
- ARP is needed because each protocol layer has its own names and addresses. An IP address is a network-level address. It describes a host, a unique destination at the network layer. A link address, in contrast, describes a particular network card, a unique device that sends and receives link layer frames
- Ethernet, for example, has 48 bit addresses. Whenever you buy an Ethernet card, it’s been preconfigured with a unique Ethernet address. So an IP address says “this host”, while an Ethernet address says “this Ethernet card.”
- 48-bit Ethernet addresses are usually written as a colon delimited set of 6 octets written in hexidecimal, such as 0:13:72:4c:d9:6a as in the source, or 9:9:9:9:9:9 as in the destination. Also call **MAC Addresses** (Media Access Control Address)

<details>
  <summary> Summary </summary>

![Example Addressing](./../../images/example-address.png)

</details>

Let’s say node A, on the left, wants to send a packet to node B, on the right. It’s going to generate an IP packet with source address 192.168.0.5 and destination address 171.43.22.5.Node A checks whether the destination address is in the same network. The netmask tells it that the destination address is in a different network: 255.255.255.0. This means node A needs to send the packet through the gateway, or 192.168.0.1. To do this, it sends a packet whose network-layer destination is 171.43.22.5 but whose link-layer destination is the link layer address of the gateway. So the packet has a network layer destination 171.43.22.5 and a link layer destination 0:18:e7:f3:ce:1a. The network layer source is 192.168.0.5 and the link layer source is 0:13:72:4c:d9:6a.So we have an IP packet from A to B, encapsulated inside a link layer frame from A to the left gateway interface. When the packet reaches the gateway, the gateway looks up the next hop, decides it’s node B, and puts the IP packet inside a link layer frame to B. So this second IP packet from A to B is inside a link layer from from the right gateway interface to B.

#### Example Problem:

<details>
  <summary> Summary </summary>

![Example problem](./../../images/example-problem-arp.png)

</details>

So here we get to the problem ARP solves. My client knows that it needs to send a packet through the gateway that has IP address 192.168.0.1. To do so, however, it needs to have the link layer address associated with 192.168.0.1. How does it get this address? We somehow need to be able to map a layer 3, network layer, address, to its corresponding layer 2, link layer, address. We do this with a protocol called ARP, or the Address Resolution Protocol.

So, ARP

- Generates mappings between layer 2 and layer 3 addresses
  - Nodes cache mappings, cache entries expire
- Simple request-reply protocol
  - “Who has network address X?”
  - “I have network address X.”
- Request sent to link layer broadcast address
- Reply sent to requesting address (not broadcast)
- Packet format includes redundant data
- Request has sufficient information to generate a mapping
- Makes debugging much simpler•No “sharing” of state: bad state will die eventually

ARP is a simple request-reply protocol. Every node keeps a cache of mappings from IP addresses on its network to link layer addresses. If a node needs to send a packet to an IP address it doesn’t have a mapping for, it sends a request: “Who has network address X?” The node that has that network address responds, saying “I have network address X.” The response includes the link layer address. On receiving the response, the requester can generate the mapping and send the packet.

So that every node hears the request, a node sends requests to a link layer broadcast address. Every node in the network will hear the packet.

Furthermore, ARP is structured so that it contains redundant data. The request contains the network and link layer address of the requestor. That way, when nodes hear a request (since it’s broadcast), they can insert or refresh a mapping in their cache. Nodes _only_ respond to requests for themselves. This means, assuming nobody is generating packets incorrectly, the only way you can generate a mapping for another node is in response to a packet that node sends. So if that node crashes or disconnects, its state will inevitably leave the network when all of the cached mappings expire. This makes debugging and troubleshooting ARP much easier.

So how long do these dynamically discovered mappings last? It depends on the device: some versions of Mac OSX, for example, keep them around for 20 minutes, while some Cisco devices use timeouts of 4 hours. The assumption is that these mappings do not change very frequently

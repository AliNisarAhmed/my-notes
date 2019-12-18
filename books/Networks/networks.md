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

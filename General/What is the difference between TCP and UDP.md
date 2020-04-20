# TCP vs UDP

## TCP (Transmission Control Protocol)
Is connection-oriented, and a connection between client and server is established (passive open) before data can be sent. Three-way handshake (active open), retransmission, and error-detection adds to reliability but lengthens latency. TCP employs network congestion avoidance. However, there are vulnerabilities to TCP including denial of service, connection hijacking, TCP veto, and reset attack. For network security, monitoring, and debugging, TCP traffic can be intercepted and logged with a packet sniffer.

TCP guarantees the recipient will receive the packets in order by numbering them. The recipient sends messages back to the sender saying it received the messages. If the sender does not get a correct response, it will resend the packets to ensure the recipient received them. Packets are also checked for errors. TCP is all about this reliability — packets sent with TCP are tracked so no data is lost or corrupted in transit. This is why file downloads do not become corrupted even if there are network hiccups.

## UDP (User Datagram Protocol)
Uses a simple connectionless communication model with a minimum of protocol mechanisms. UDP provides checksums for data integrity, and port numbers for addressing different functions at the source and destination of the datagram. It has no handshaking dialogues, and thus exposes the user's program to any unreliability of the underlying network, there is no guarantee of delivery, ordering, or duplicate protection.

UDP is suitable for purposes where error checking and correction are either not necessary or are performed in the application. UDP avoids the overhead of such processing in the protocol stack. Time-sensitive applications often use UDP because dropping packets is preferable to waiting for packets delayed due to retransmission, which may not be an option in a real-time system.

| TCP| UDP |
|---|---|
| Guarantees delivery of data to the destination router | Delivery of data is not guarantess |
| Provides extensive error checking mechanisms. It is because it provides flow control and acknowledgment of data | Has only the basic error checking mechanism using checksums |
| Sequencing of data, packets arrive in-order at the receiver | There is no sequencing of data. If ordering is required, it has to be managed by the application layer |
| Retransmission of lost packets is possible | There is no retransmission of lost packets |
| Has a (20-80) bytes variable length header | Has a 8 bytes fixed length header |
| Doesn’t supports Broadcasting | Supports Broadcasting |
| Used by HTTP, HTTPs, FTP, SMTP and Telnet | used by DNS, DHCP, TFTP, SNMP, RIP, and VoIP |

## Links
https://www.guru99.com/tcp-vs-udp-understanding-the-difference.html    
https://www.geeksforgeeks.org/differences-between-tcp-and-udp/    
https://www.privateinternetaccess.com/blog/tcp-vs-udp-understanding-the-difference/   
https://support.holmsecurity.com/hc/en-us/articles/212963869-What-is-the-difference-between-TCP-and-UDP-   
https://en.wikipedia.org/wiki/User_Datagram_Protocol  
https://en.wikipedia.org/wiki/Transmission_Control_Protocol  

# 1. Why can't TCP do broadcasts?

- TCP is a connection based transport protocol, it binds itself to a IP and port, and can only perform unicasts

# 2. How does TCP recover from lost packets?

- TCP has a `Retransmission Time-Out` (**RTO**) mechanism that after the time-out occurs we retransmit the packet

![](https://assets.extrahop.com/images/infographics/TCP-RTO-Retransmission-Timeout-Diagram.jpg)

- TCP also recovers from lost packets by **receiving multiple ACKs** for a segment that was not received
    - The server keeps sending the ACK for the _last received segment_ until the client resends the lost segment

![](https://accedian.com/wp-content/uploads/2018/09/Duplicate-ACK-Performance-Vision.png)

# 3. How does it know that it lost packets?

- Since packet retransmission is only a client-only activity, there 2 causes:
    - The client expects an ACK, but does not receive it <packet or ACK lost> (retransmission)
    - The client receives duplicate ACK, and then retransmits the lost packet

## 3.1 How does it know how long to wait?

- TCP has a `Retransmission Time-Out` **RTO** and it symbolizes the window for TCP to initialize a retransmit

## 3.2 Why is it not simply the mean RTT? (you don't need to know all the formulas to calculate RTO)

- The **RTO** should be longer than RTT, but NOT too long (slow reaction to segment loss)

- If its **too-short** we create a premature time-out and causes unnecessary retransmissions

# 4. How does a receiver detect a corrupt packet?

1. _Incorrect packet order_: Since TCP uses sequence numbers, a wrong ordering of the packets, will cause retrainsmission

2. Packet loss

3. Corrupt data inside the packet
    - Both receiver and sender calculate the `checksum`, and compare the values to spot if the data has been corrupted

# 5. How do receivers detect duplicate packets?

- The receiver stores the sequence numbers of the last packet received in order, plus a list of packets that have arrived out of order.

# 6. What is the problem with only sending a single packet at a time and waiting for an ACK?

- Only sending 1 packet at a time wastes bandwidth, and we don't use our assigned sliding window portion

- TCP has the ability to send accumulated ACKs based on the last-received packet

## 6.1 Calculate the maximum possible bandwidth if the packet size is 1KB and the round trip time is 22ms.

![](https://latex.codecogs.com/gif.latex?1%5C%20%5Ctext%7BKB%7D%20%3D%201024%20%5C%20%5Ctext%7Bbytes%7D%20%5C%5C%20%5C%5C%2022%20%5C%20%5Ctext%7Bms%7D%20%3D%2022*10%5E%7B-3%7D%20%5C%20%5Ctext%7Bseconds%7D%20%5C%5C%20%5C%5C%20%5Ctext%7BBandwidth%7D%20%3D%20%5Cfrac%7B1024%20%5C%20%5Ctext%7Bbytes%7D%7D%7B22*10%5E%7B-3%7D%20%5C%20%5Ctext%7Bseconds%7D%7D%20%3D%2045.%5Cbar%7B45%7D)

## 6.2 What if the packet size is 512 bytes and 20ms.

![](https://latex.codecogs.com/gif.latex?%5C%5C%20512%20%5C%20%5Ctext%7Bbytes%7D%20%5C%5C%2020%20%5C%20%5Ctext%7Bms%7D%20%3D%2020*10%5E%7B-3%7D%20%5C%20%5Ctext%7Bseconds%7D%5C%5C%20%5C%5C%20%5Ctext%7BBandwidth%7D%20%3D%20%5Cfrac%7B512%20%5C%20%5Ctext%7Bbytes%7D%7D%7B20%20%5C%20%5Ctext%7Bms%7D%20%3D%2020*10%5E%7B-3%7D%20%5C%20%5Ctext%7Bseconds%7D%7D%20%3D%2025%20%5C%20%5Ctext%7BKB/s%7D)

# 7. How do sliding windows help the maximum bandwidth problem?

- Sliding windows makes us take advantage to send the most packets within the window

# 8. Why do we use a sliding window?

- **Sender Window Size**: It helps keeping track of which sequence numbers have already been acknowledged, which are in flight, and which are yet to be sent.

![](https://raw.githubusercontent.com/aldogutierrez/NetworkManagementToolkit/master/pictures/senderWindowTCP.png)

- **Receiver Window Size**: It helps the receiver know which sequence number to expect next
    - Sequence numbers are stored when received, and those outside the window are discarded

![](https://raw.githubusercontent.com/aldogutierrez/NetworkManagementToolkit/master/pictures/receiveWindowTCP.png)

# 9. How do initial window sizes and timeouts get chosen?

- The application can advertise a window size based on its performance and environment

# 10. How do we pick a window size?

- According to the `End-To-End Principle` we don't really know how reliable the data transfer so we just guess
    - `Dropped Packet Detection`: We send more packets to the window **until** the network drops some packets, then we slow down the packet transfer. Then we use the maximum-non-dropped packet size.

#11. How does the window size change?

- When the network load becomes too big, we need to lower the window size so that we don't overload the network


![](https://cdn.networklessons.com/wp-content/uploads/2015/07/xtcp-global-synchronization.png.pagespeed.ic.97a6qm9Ogu.png)

# 12. How does a sender know that it is sending too fast?

- Their re-transmission rate go up, and the sender is not receiving the ACKs they were expecting
    - This happens when the receiver has a full buffer and cannot process the segments sent by the sender

# 13. How is sending a packet different when using UDP than TCP?

- **UDP**: The sender sends straight to the receiver, there is no handshaking, there is no time-out, if a packet gets lost, it is the responbility of the sender to re-transmit

- **TCP**: First, the sender & receiver stablish a connection via handshaking, a time-out is set for the connections in case there is a need to re-transmit, in case the packet gets lost, TCP uses the `double ACK` or the `time-out`

# 14. How is data packetized in TCP?

- Since TCP transports application data (POP3 <email>, HTTP <webpages>, FTP <files>) and splits them into chunks that TCP thinks are the best way to transport them. 

- TCP must convert a sending application’s stream of bytes into a set of packets that IP can carry.
    - Typically fitting each segment into a single IP-layer datagram that will not be fragmented

- E.g

10KB packet sent by the client

However, the MTU is 1300 bytes

TCP header = 40 bytes
Then, we can only send (1300-40) = 1260 bytes

**TCP send is a stream of bytes --> are broken into smaller chunks for easy transfer**

So 10KB = 10240 bytes

10240 bytes / MTU = 8 packets of 1300 bytes

At the end we will send a 160 byte packet, then we have transfered the 10KB file

# 15. What do the different fields in TCP header represent?

![](https://raw.githubusercontent.com/aldogutierrez/NetworkManagementToolkit/master/pictures/tcpHeader.jpg)

- Source Port: From what port am I sending
- Destination Port: To where I want to send
- Sequence Number: Starts at 1 - ends at the length of the segment
- Acknowledgement Number: Sequence Number + Segment Length
- Data Offset: 
- Control Flags: TCP header specific
    - Reserved
    - Nonce, Urgent, Syn Fin, etc...: Info flags for data exchange
- Window Size: How many packet could be sent
- Checksum: For integrity purposes
- Urgent Pointer: ?

# 16. What do the different states of TCP mean? In other words, if you do a "netstat -an" and see that state, what do you know about the connection?

![](https://raw.githubusercontent.com/aldogutierrez/NetworkManagementToolkit/master/pictures/TCPstateDiagram.png)

# 17. What is a half-open connection? What can it be used for?

- A `half-open` connection happens when the host at one end of the TCP connection has crashed, or has otherwise removed the socket without notifying the other end.

- If the remaining end is idle, the connection may remain in the half-open state for unbounded periods of time.

- `Half-Open` **BELONGS TO** `SYN-SENT`, `SYN-RECEIVED`
    - A `half-open` connection goes back to a `LISTEN` state for a formal `RESET`

- Such connections will automatically become reset if an attempt is made to send data in either direction

- Does **NOT** belong to `ESTABLISHED`, `FIN-WAIT`, `CLOSE-WAIT`, `CLOSING`, `LAST-ACK`, `TIME-WAIT`

# 18. What does the sequence number represent in TCP?

- Sequence numbers represent some kind of packet ID, to help recover from damaged, lost, duplicated or ou-of-delivery packets

- For TCP data transfer to suceed, each `sequence number` **must** be positively ACKed

# 19. What is the first sequence number of a TCP connection?

- The Initial Sequence Number `ISN` works as a unique TCP connection identifier
    - It can be any number between 0 and 4,294,967,295

# 20. When a sequence number is ACKed, what does that mean?

- The packet that was transmitted was sent succesfully, i.e received by the receiver

## If a receiver ACKs sequence 27 and then ACKs sequence 1022, but the ACK 27 gets lost, does it need to resend the ACK for 27?

- NO

- TCP likes to send cummulative ACKs so it does **NOT** need to send an ACK packet for each sequence number

# 21. What does it mean that TCP is full-duplex?

- A `full-duplex` connection can only connect 2 devices

- A `full-duplex` connection between two devices is capable of sending data in both directions simultaneously

- It is constructed by using 2 `simplex-links`: 1 for sending, 1 for receiving OR 1 `bi-directional link`
    - `Simplex-link` = `one-way street`

## 21.1 Why do you need to calculate a window size for each direction of a TCP channel?

- Because **each device has different buffer capabilities**
    - A smartphone has less buffer space than a server

- In most cases, upload is faster than download, so both sending & receving buffers must be different

# 22. What are selective ACKs?

- Allows the receiver to indicate to the sender that it has received the `out-of-order` data correctly

- It happens when:
    - The sender sends out-of-order packets
    - The receiver **DOES NOT** ACK the packets, so that the sender re-transmits in-order
    - Once the sender sends the packets in the correct order
        - The receiver ACKs the _new_ in-order packets that were sent out-of-order previously

# 23. Why would you use the window scaling option?

- Potentially, to increase the `receive window` size and to increase throughput

- The `window scale option` must be agreed by both sender & receiver in their `SYN`

# 24. How is a connection uniquely identified by a server?
(In other words, when a TCP packet is received for example on port 80 and there are thousands
of connections to that port, how does the server know which connection the packet is for?)

- A sequence number is randomly-chosen for identification purposes
    - A sequence number & ACK number switch

- A TCP connection is identified by a quadruple tuple `(source IP, source port, destination IP, destination port)`

# 25. What do congestion control algorithms do? Name three of them.

- **Slow Start**:

![](https://www.keycdn.com/img/support/tcp-slow-start.png)

![](https://upload.wikimedia.org/wikipedia/commons/2/24/TCP_Slow-Start_and_Congestion_Avoidance.svg)

- **TCP Tahoe**: Everytime we overload the network, the transmission rate is plummetted to 1, and then restart the `slow start`

- **TCP Reno**: Everytime we overload the network, the transmission rate plummets to the `threshold` value, instead of 1; however, once we get more duplicate ACKs, we restart the `slow start`

![](https://upload.wikimedia.org/wikipedia/commons/thumb/b/be/CongWin_in_TCP_Tahoe_e_Reno.png/800px-CongWin_in_TCP_Tahoe_e_Reno.png)

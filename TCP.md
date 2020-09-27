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

1KB = 1024 bytes

22ms = 0.022 sec

1024/0.022 = 46545.45 bytes/s = 45.45 KB/s

## 6.2 What if the packet size is 512 bytes and 20ms.

512/0.02 = 25600 bytes/s = 25 KB/s

# 7. How do sliding windows help the maximum bandwidth problem?

- Sliding windows makes us take advantage to send the most packets within the window

# 8. Why do we use a sliding window?

- **Sender Window Size**: It helps keeping track of which sequence numbers have already been acknowledged, which are in flight, and which are yet to be sent.



- **Receiver Window Size**: It helps the receiver know which sequence number to expect next
    - Sequence numbers are stored when received, and those outside the window are discarded



# 9. How do initial window sizes and timeouts get chosen?

- The application can advertise a window size based on its performance and environment

# 10. How do we pick a window size?



#11. How does the window size change?



# 12. How does a sender know that it is sending too fast?

# 13. How is sending a record of data different when using UDP than TCP?

# 14. How is data packetized in TCP?

# 15. What do the different fields in TCP header represent?

# 16. What do the different states of TCP mean? In other words, if you do a "netstat -an" and see that state, what do you know about the connection?

# 17. What is a half-open connection? What can it be used for?


#18. What does the sequence number represent in TCP?

#19. What is the first sequence number of a TCP connection?

# 20. When a sequence number is ACKed, what does that mean?

## If a receiver ACKs sequence 27 and then ACKs sequence 1022, but the ACK 27 gets lost, does it need to resend the ACK for 27?

# 21. What does it mean that TCP is full-duplex? Why do you need to calculate a window size
for each direction of a TCP channel?

# 22. What are selective ACKs?

# 23. Why would you use the window scaling option?

# 24. How is a connection uniquely identified by a server?
(In other words, when a TCP packet is received for example on port 80 and there are thousands
of connections to that port, how does the server know which connection the packet is for?)

# 25. What do congestion control algorithms do? Name three of them.




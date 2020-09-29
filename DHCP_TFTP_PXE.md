# 1. What is the relationship between BOOTP and DHCP?

- BOOTP acted as an earlier version protocol for IP address assignment.

- Then, DHCP came along, and added options to their protocol, the options can include DNS server and DHCP relay.

## 1.1 How do they differ?

- In DHCP, when the server received a `DISCOVER` packet the client can request several **options** that are helpful to the client.
    - Some options include: 

# 2. How do options generalize the functionality of DHCP?

- When a client sends an option, and it's repeated, the DHCP server concates them in a single message so that the flags are not repeated

# 3. What are some DHCP options that you see when booting the Pi?

- DISCOVER and OFFER

# 4. Why do you think DHCP is based on UDP?

- It is because UDP is connection-less and you can broadcast with UDP.

# 5. What are the phases of DHCP? How do the packet formats differ?

## 5.1 Allocation/Lease

- **DISCOVER**:

![](https://www.netmanias.com/en/?m=attach&no=3368)

- **OFFER**:

![](https://www.netmanias.com/en/?m=attach&no=3369)

- **REQUEST**:

![](https://www.netmanias.com/en/?m=attach&no=3370)

- **ACK**:

![](https://www.netmanias.com/en/?m=attach&no=3371)

## 5.2 Renewal

- **REQUEST**:

![](https://www.netmanias.com/en/?m=attach&no=3372)

- **ACK**:

![](https://www.netmanias.com/en/?m=attach&no=3373)

## 5.3 Release

- **RELEASE**: 

![](https://www.netmanias.com/en/?m=attach&no=3374)

# 6. How do the functional requirements of DHCP differ from IPv4 and IPv6?

- IPv4 and IPv6 have 2 different modes, stateful and stateless
    - IPv4 is only stateful
- IPv6 offers both statful and stateless modes
    - Stateful works just like DHCPv4
    - Stateless only deals with options

# 7. What is a DHCP relay and why is it needed?

- A DHCP relay acts like a messenger for the actual DHCP server <middleman>

- Sometimes a router drops broadcasts messages so the DHCP server never gets the request

- The DHCP relay, acts like a middleman to handle broadcasts and sends them to the ACTUAL DHCP server

## 7.1 Why do we really only need the DHCP relay for handling the broadcast requests?

- The DHCP relay changes the bradcast address to the actual DHCP server address, then the router can send the request to the server


Client: 0.0.0.0 -> DHCP Discover -> 255.255.255.255 <broadcast>

DHCP Relay IP: 1.1.1.254 <received all broadcasts>: 0.0.0.0 -> DHCP Discover -> 100.1.1.1

Server: IP: 100.1.1.1 -> DHCP Offer ->  172.68.31.5

- Once the Lease/Allocation is completed, the client would know the actual IP of the server, so all RENEWAL, RELEASE requests would go directly to the DHCP server

# 8. When would you use a proxy DHCP server? How it is different from a DHCP relay?

- We would use a DHCP Proxy to keep the IP of the ACTUAL DHCP server hidden. This would prevent attacks to the DHCP server

- DHCP proxy would act as a middleman for ALL DHCP requests, including Leasing/Allocation, Renewal, Release

- DHCP Relay would separate client from server

- DHCP Proxy uses the DHCP Server Identifier (Option 54)

![](https://www.netmanias.com/en/?m=attach&no=3402)

# 9. Why is TFTP needed if we already had FTP? Why do you think PXE uses TFTP rather than FTP?

- TFTP is easier, requires NO authentication, NO connection (as clients should handle packet retransmission)

- FTP uses TCP and TFTP uses UDP

- PXE Boot offers the most simple booting

# 10. There is no security in TFTP at all, so why would you ever want to enable writing with TFTP?

- TFTP offers a very simple way to download and write files, however because there is no security, we can limit the use of TFTP on users with certain IP addresses (being part of a trusted LAN)

- The files we want to write to are stored in a Secure File Transfer Server, and once authenticated we can use TFTP

# 11. What does the sorcerer's apprentice have to do with TFTP?

- So, since the client is responsible for hanlding its own re-transmissions, the SAS happens when a data block is `delayed` rather than `lost`

- The client would re-transmit the block number as it timed out for the ACK for such block

- The server would resend the ACK to the client

- So now EVERY SINGLE packet is transmitted twice

- **NOTE** If file is _small_ the transfer would terminate fine, but if the file is _long_ we could potentially create a network congestive collapse

# 12. How does Wireshark know the manufacturer of the card sending out network traffic?

- NICs (Network Interface Cards) are made by a specific company, and these cards have the MAC address burnt into them, so Wireshark would know the ranges of MAC addresses and respectively their manufacturer.

## 12.1 Where does it get the information from?

- Wireshark uses the `Wireshark Manufacturer Database` it contains most of the manufacturers domains for MAC addresses


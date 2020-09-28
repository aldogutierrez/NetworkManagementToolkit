# 1. What are the two kinds of Ethernet packets? How do you know which is being used?

- Ethernet <Regular, IEEE 802.3>: Only has a maximum payload of 1500, has src and dest MAC addressses

- IEEE 802.11 <WiFi>: Has a maximum payload of 2304, and has **4** addresss fields, src, dest, router, and access point

![](https://cecs.wright.edu/~pmateti/Courses/4420/Lectures/Refreshers/ethernet-wifi.jpg)

# 2. What do each of the fields of Ethernet mean?

- **Preamble**: Establishes **bit synchronization**
    - 7-bytes

- **Start of Frame Delimiter**: Only 1-byte in length, always set to `10101011`
    - 1-byte

- **Destination Address**: The destination's MAC address
    - 6-bytes

- **Source Address**: The source's MAC address
    - 6-bytes
    - The LSBit of the 1st byte is set to 0

- **Length**: Represents the entire length of the frame
    - 2-bytes 

- **Data**: Actual data is inserted, also known as the **PAYLOAD**
    - Has a Maximum Tranmission Unit of `1500` bytes
    - Padding of 0s is included if the length is less than 46 bytes

- **CRC**: Hash code representing the data 
    - 4-bytes
    - Hash(Dest Addrr, Src Addr, Length, Data Field)

# 3. Where do MAC addresses come from?

- They're burnt into the _Network Interface Card_ (NIC) provided by the manufacturer and managed by the IEEE

# 4. Why do we need IP? Why can't we just use MAC addresses instead of IP addresses?

- MAC addresses are used to ensure the physical address of the computer

- For the IP address identifies the client their connection to the network

# 5. How do we know if we get an IPv4 or IPv6 packet from the network?

- We would know simply because the packet length is larger, because IPv6 addresses are 128-bits long

- The IPv6 headers are simpler, as only the `version` and the `source` and `destination addressses` are kept.

- IPv4 contains a `Time To Live`, `Checksum`, and `Fragmentation`
    `CRC`: Hash code representing the data and the `payload` length would vary
 
- This would show how dynamic the headers are

# 6. Where do IPv4 addresses come from?

- IP addresses are generated in the Network Layer and managed by the DHCP protocol

- The **Internet Assigned Numbers Authority** (IANA) assigns blocks of IP addresses to regional internet registries. In turn, these regional registries allocate addresses to ISP, companies, schools, and similar institutions within their zone.

# 7. What is a subnet mask?

- A subnet mask is the range in which the network address is contained within the full IP address

- To know how long the network address is, we can look at the **CIDR** notation and look for the numbers after the `/`
    - `CIDR` stands for **Classless Inter-Domain Routing**

# 8. How do you get the network address from an IP address?

- Say we have an IP address of `172.68.31.5/24`
    - According to the **CIDR** notation, the first 24-bits belong to the network, the rest (the last 8) belong to the host
    - Therefore, the network address is `172.68.31.0`
    - The host is `0.0.0.5`
    - The subnetmask would be `255.255.255.0`

# 9. How do you get the broadcast address from an IP address?

- Broadcast Address = Network Address + Flood the Rest with 1's

- E.g `What is the broadcast address for 192.168.1.150/25`
    - The network address has the 1st bit of the last block
    - 150 in binary is `1`0010110
    - The network address is `192.168.1.128`
    - When flooding everything else with 1's the result is `192.168.1.255`

# 10. What are the fields of an IPv4?

![](https://www.researchgate.net/profile/Muzhir_Al-Ani/publication/269810379/figure/fig1/AS:295073662160901@1447362451826/Comparison-of-IPv4-and-IPv6-headers-structures-15.png)

# 11. UDP doesn't provide much additional functionality for IP. Why do we need it? Why not just use IP?

- IP is part of the `network` layer while UDP is part of the `transport` layer

- When systems use Network Address Translation (NAT) the network can be represented by 1 IP address so we would need another type of identificaiton for packet sending, so we use the `port number`.

# 12. How does UDP checksum break layering?

- The UDP checksum includes IP header information in the checksum, which is at a lower layer (IP).

## 12.1 Why is it done?

- UDP does this to ensure that the packet arrived at the expected destination. Moreover, it will also make sure that the replies are going back to the expected source

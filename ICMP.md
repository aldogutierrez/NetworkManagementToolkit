# 1. What layer does ICMP run at?

- **Sending (source) layer**: Network (L3)
- **Receiving (destination) layer**: Transport (L4)
- **Often times**: Application (L7)

----------

- ICMP IPv4 Packet

![](https://raw.githubusercontent.com/aldogutierrez/NetworkManagementToolkit/master/pictures/icmp_for_ip.png)

- ICMP fixed header

![](https://raw.githubusercontent.com/aldogutierrez/NetworkManagementToolkit/master/pictures/icmp_fixed_header.png)


# 2. How does `ping` work?

- `ping` makes echo requests (**type 8**), and receives echo responses (**type 0**)
- `ping` is simply sending some characters (mostly empty) and the response is just sending the same thing back
- You mostly use `ping` to check if a host is alive or not
- `ping` uses `ICMP` heaviliy, as `ping` is a type of `ICMP` message

# 3. How does tracepath work?

- `tracepath` is a tool that follows packets through a network & reports back the information about the packet's journey (including **networks**, **switches**, **hops**, etc.)
- `tracepath` does not require *super-user privileges*, and discovers the *maximum transmission unit* (MTU) along with the path
- `tracepath` also makesuse of `ICMP`
- `tracepath` changes the *time-to-live* on every *hop*, that is it subtracts one, and if it reaches zero, it is reported back

# 4. How can you transmit data with ICMP?

- In the last section of the ICMP packet, theres a variable field to input data and transmit it using `ping` for example.

# 5. Look at the ICMP message types 3, 4, 5, 11. What are they used for?

- `3`: ICMP notifies with message type `3` for **_Destination Unreachable_**
    - Seen in `ping`

- `4`: ICMP notifies with message type `4` for **_Source Quench - congestion_**
    - Happens if a router or host does not have sufficient buffer space to process the request

- `5`: ICMP notifies with message type `5` for **_Redirect - Indicates that an alternate router should be used_**

- `11`: ICMP notifies with message type `11` for **_Time Exceeded - TTL Resource exhausted_**
    - Seen in `tracepath`

# 6. Look at the first 4 subcodes of the ICMP message type 3. Come up with a scenario which you would see errors with each of those subcodes.

- Type `3` Code `0`: **_Net Unreachable - No route (at all) to destination_**
    - We see this message when we try to send a packet through the internet to an IP address that does not exist, or there's a misconfiguration in the router settings

- Type `3` Code `1`: **_Host Unreachable - Known route but unreachable host_**
    - We see this message when the host that you we trying to ping is down (it HAS an IP address) or is not operating on the network (maybe under maintenance)

- Type `3` Code `2`: **_Protocol Unreachable - Unknown (transport) protocol_**
    - We see this message when we use another transport protocol besides TCP and UDP is used

- Type `3` Code `3`: **_Port Unreachable - Unknown/unused (transport) port_**
    - We see this message when an incoming datagram is destined for an application that is not ready to receive it; OR when a message is sent to a port number that is not in use by any server process
        - Happens when we try to use TFTP on a server that does not provide TFTP services (port 69 is unused and closed)

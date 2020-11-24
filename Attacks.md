# 1. How can DHCP be used to find information about a network?

- Within the DHCP DISCOVER packer there are options that can be requested upon server discovery
- Some of these options can be:
    - `Option 1`: Subnet mask to be applied on the interface asking for an IP address
    - `Option 3`: Default router or last resort gateway for this interface
    - `Option 6`: Which DNS (Domain Name Server) to include in the IP configuration for name resolution
    - `Option 51`: Lease time for this IP address

- From Wireshark Capture:

![](https://raw.githubusercontent.com/aldogutierrez/NetworkManagementToolkit/master/pictures/dhcp_wireshark.png)

# 2. What is a stealth scan?

- Stealth scans are part of a probing attack
    - It consists of trying to establish TCP connections with random ports that provide services

- Steps:
    1. Send a `SYN` packet
    2. Receive a `SYN/ACK` packet
    3. At this point you send a `RST` packet to terminate connection attempt (IF AND ONLY IF you receive a `SYN/ACK` first)
        - By sending a `RST` packet, **we prevent the application/machine from logging our attempt** of connection and its intent

- In case we get an `RST` packet instead of a `SYN/ACK` packet it means that the port is **closed**

- If we do not get a response back, or if we get ICMP error packets, it means that the port is **filtered**

**_NOTE_**: Stealth scanning can only be done with root access within the network

# 3. How are UDP ports scanned?

- With comparison to stealth scanning, UDP cannot be used because UDP is connection-less and does not have states

- However, to scan UDP ports all we have to do is just send a packet, and wait for a response
    - We can even send an empty packet
    - `nmap` is a tool that can be used for UDP port scanning

- From an `nmap` capture

![](https://raw.githubusercontent.com/aldogutierrez/NetworkManagementToolkit/master/pictures/nmap_capture.png)

## 3.1 How come it may be difficult to distinguish between an open and filtered UDP port?

- `open` vs `filtered` ports:
    - `open`: this means that the port is listening for any activity
    - `filtered`: this means that the port is blocking conversations just like a firewall or router drops unwanted traffic

- When probing for open UDP ports, it may be difficult to decide if a port is open or filtered because of the response we get
    - _If we get a packet response_ (rare), the port is **open**
    - _If we don't get a reponse_ (even after retransmitting), the port may be **open** or **filtered**
        - The port may be **open** after receiving our packet and _is expecting something else_ (we don't know)
        - The port may be **filtered** after receiving our packet and then _dropping it_
    - _If we get an ICMP error packet_, the port may be **closed** or **filtered**

# 4. What is a "Xmas tree" scan?

- A "xmas tree scan" consists of sending a TCP packet with **_ALL flags turned on_**
    - These flags are `FIN`, `PSH`, and `URG`
    - Just like turning on all Christmas lights
- The responses we might get back will vary according to the machine's operating system

![](https://static.packt-cdn.com/products/9781788995177/graphics/e2350deb-c2bc-45bc-90a1-9761fc13c910.png)

# 5. What is the network fingerprint of an OS?

- By OS Fingerprinting we are referring to the default fields that are received in a xmas scan
    - TTL
    - Window Size
    - Packet Sice
    - Don't Fragment
    - Terms of Service

# 6. How is ARP used to do a man-in-the-middle attack?

- What ARP does is that it **_updates the IP address to MAC address resolutions_** between hosts on the network.

- If _we are the attacker_, we can broadcast our MAC address to the machines in the network (just like a gratuitous packet).
    - We can tell the victim that we are the router
    - We can tell the router that we are the victim

![](https://www.imperva.com/learn/wp-content/uploads/sites/13/2020/03/thumbnail_he-ARP-spoofing-attacker-pretends-to-be-both-sides-of-a-network-communication-channel.jpg)

- However, this won't last forever as the ARP cache will eventually become invalidated.
    - **_ARP cache is time-based_**, which the router and the system will renew their cache
    - While ARP is cached-based, **_any of these hosts can broadcast their MAC again_**, without the ARP cache being invalidated

# 7. Why does an ARP attack need to be carried out on a local network?

- ARP fundamentally works using broadcasts
- **Routers do not like internet broadcasts**
    - If the attacker was trying to attack a non-local network the routers would just drop the packet, as sending the ARP broadcast to ALL networks is impossible, the router would just drop it.
- However, routers can send broadcasts on the same network as that is more manageable

# 8. What are other ways of carrying out a man-in-the-middle attack?

- Once we are on a network, or barely on a network, we can turn on `promiscuous mode`
    - **Promiscuous mode** listens to all the packets in the network, the ones that it is programmed to receive and even those that are not
        - _We can learn a lot about the network and we are listening to everything_. We can see broadcasts, which IPs are active, we can see MAC addresses (tells us which devices are active), etc...

- We can also put ourselves as a man-in-the-middle by convincing other hosts that we are the DHCP server
    - All we have to do, is answer faster than the DHCP server
        - The attacker will include its IP in the DHCP OFFER packet and make itself appear as the router and DNS server in the options for DHCP

![](https://raw.githubusercontent.com/aldogutierrez/NetworkManagementToolkit/master/pictures/dhcp_mitm.png)

# 9. What are the "packets of death"? ("ping of death" being the most famous?)

- The `packets of death` are known to use ICMP or UDP, and it exploits the end-to-end principle, which bypases router and firewall checks

- **Packets of death** send a packet so massive that the host does not have enough buffer space and overflows
    - **Best case scenario**: crashes the machine and just restarts
    - **Doomsday scenatio**: You can overflow critical code that is in memory ready to be executed by the CPU and delete/attack components, also known as `remote code execution`

![](https://www.ionos.com/digitalguide/fileadmin/DigitalGuide/Schaubilder/how-a-ping-of-death-works.png)

- Ping of death occurs when the size of the packet exceeds 64K

![](https://raw.githubusercontent.com/aldogutierrez/NetworkManagementToolkit/master/pictures/udp_fragmentation.png)

# 10. How can you DoS a DHCP server?

- **Denial Of Service** attacks are successful when a server providing a service is overloaded and no longer able to fulfill service requests

- The way we can overload DHCP servers is to just keep asking for IP addresses
    - For each request you can change the source MAC address and the DHCP server won't care, it will give you an address no matter what, because its is job
    - You can generate as many MAC addresses as you want until the DHCP server exhausts the avaible IPs

![](https://raw.githubusercontent.com/aldogutierrez/NetworkManagementToolkit/master/pictures/dhcp_dos.png)

## 10.1 How is it similar to the method to DoS TCP?

- A **TCP DoS** attack is similar to the DHCP DoS in a way that we exhaust all the possible connections that can be made to the server on a specific port
    - The attacker just keeps sending `SYN` packets to a server
    - The server will respond with `SYN/ACK` packets
    - The attacker does nothing (doesn't respond)
    - The server (by nature) sets a timer for the response, but it won't get any
        - Then the server becomes busy with empty requests
    - Users trying yo connect to the server won't be able to because it's completely busy

![](https://miro.medium.com/max/800/1*OxbEjbK8qyb6J_lYZ4BLLQ.png)

# 11. Why is it hard to tell if your firewall is configured incorrectly?

- The firewall rules (such as `MASQUERADE`) will allow incoming connections from unknown sources to pass through; however, you may think this is normal because all your packets will go through the firewall without problems
    - _The real problem is not your connections going out, but those coming in_

# 12. How is an address spoofing used to carry out a Distributed Denial of Service (DDoS) attack?

- **The attacker can use other hosts to be part of the attack by using their IP address** to overload the service
    - The attacker's IP won't be blocked because they're not using their own IP, they're using other host's IPs
        - Potentially, **the attacker uses throusands of machines to overload the server**

![](https://www.f5.com/content/dam/f5-labs-v2/article/articles/edu/20190605_what_is_a_ddos/DDoS_attack.png)

# 1. What is NAT?

- **Network Address Translation**

- NAT _changes network addresses_ when they are _passed through a router_
    - There exists a **_1:1 ration from internal addresses to external addresses_** (the router takes care of that)

- Since the router is changing network addresses, _it also needs to change other fields_ from the packet so that it remains valid
    - One example is IP _checksum_, and UDP/TCP _checksum_

- While NAT is highly functional, **targets IPv4 address exhaustion**, it imposes _problems with DHCP address renewal_
    - _NAT is completely transparent/invisible_ as the host has no idea they're being translated to another network address

- When NAT is used, **_sender and receiver do not know each other's routable address_**

![](https://www.onlinecomputertips.com/images/networking/n146.jpg)

# 2. What is the difference between NAPT and NAT?

- **NAPT** (_Network Address and Port Translation_): Very much like NAT, NAPT also **changes the port number** for each host, allowing us to _reuse a single address_ for multiple hosts.
    - If NAT changed the packets quite a lot, **NAPT has to change headers even more**, as now the ports are changed in UDP or TCP

- **NAT** (_Network Address Translation_): In question #1 we discussed how NAT performs a 1:1 internal to external address exchange
    - Limits the number of public addresses to be used by a organization or company

# 3. What is the difference between DNAT and SNAT?

- **DNAT** (_Destination Network Address Translation_): DNAT is used when a packet is destined to reach a host on a private network/intranet. The NAT device, running on the router, will change the incoming packet from the server to a private IP to reach its destination

- **SNAT** (_Source Network Address Translation_): SNAT is used when a packet is destined to reach a remote host on a public network/internet. The NAT device will change the private IP of the host, to it's public IP of the router.

- NAT seems to be a combination of SNAT and DNAT

![](https://i0.wp.com/ipwithease.com/wp-content/uploads/2017/09/SNAT-VS-DNAT-1.jpg)

# 4. What does SNAT and SOCKS have in common?

- Both SNAT and SOCKS **_act as a middleman_** for us (in a private network) to change addresses
    - _SNAT_ translates it to the firewall's public IP with a port
        - When it comes back, it translates again to our private IP
    - _SOCKS_ creates a TCP connection using the firewall's public IP address and port
        - When it comes back, our private connection to the firewalls private IP would deliver the data

# 5. What is the difference between SNAT and SOCKS?

- The _SNAT_ translation happens on the _network layer_ (**L3**)

- _SOCKS_ connections happen on the _application layer_ (**L7**)

# 6. When you run `netstat -an` to look at a connection made through SOCKS vs a connection with SNAT what difference will you see?

- **Connection made through SOCKS**: To the user this would look like 2 connections
    - `Connection #1`: _INNER && PRIVATE_
        - **FROM**: Private IP behind the firewall
        - **TO**: Firewall's private IP
    - `Connection #2`: _PUBLIC_
        - **FROM**: Firewall's public IP
        - **TO**: Service public IP

- **Connection made through SNAT**: To the user this would just look like a direct connection (however, we're not aware of our address translation)
    - FROM: Private IP behind the firewall
    - TO: Server on the internet

# 7. How does SNAT work with TCP?



# 8. How does SNAT work with UDP?



## 8.1 What are the 3 filtering policies for packets coming back?

- **Endpoint-Independent Filtering**: Allows in _ANY_ incoming packet (regardless of precedence) to the private network as long as there exists a public --> private mapping
    - Allows anything if it exists

- **Address Dependent Filtering**: Allows packets from public internet connections, if and only if we (from our private network) our IP has previously contacted the public IP source on the internet
    - Allow connections `using addresses` only

- **Address and Port Dependent Filtering**: Allows packets from public internet connections, if and only if we (from our private network) have contacted the public IP source on the internet at a certain port
    - Allows connections using `address:port` only

![](https://raw.githubusercontent.com/aldogutierrez/NetworkManagementToolkit/master/pictures/3_nat_filters.png)

# 9. Why do we need DNAT?

- DNAT is used when there exists **_web services that lie behind NAT_** on their own network, and need to be accessed by users on the internet

- DNAT "completes" the picture of `PRIVATE --> PUBLIC` && `PUBLIC --> PRIVATE`

# 10. What is some information and functionality that an application using SOCKS has than an application using NAT does not?

- In SOCKS, the outgoing packets are not translated i.e. the addresses are not changed
    - SOCKS has less overhead as none of the addresses are changed

- When using SOCKS, the application **must know** about SOCKS, allowing for REQUESTS and LISTENING

# 11. How does NAT compensate for the lack of functionality and information that some specialized applications need? (FTP is a classic example)

- _FTP_ and _TFTP_ need a **LISTENING** connection to be able to keep on sending file packets _from the source_ (server) _to the destination_ (us)

- NAT has **_plugins_** that let us open a port on our system so that applications may connect to them
    - In this case, we open port `67158` on our IP `192.168.61.5`
        - NAT will then translate such address as listening/open `68.27.62.98` on port `75369`

- Once we contact a TFTP or FTP server, when it connects back, it **_will be able to connect_** to the open port where we are listening

# 12. What is IPtables?

- `iptables` are the main technique to build firewalls. In essence, `iptables` creates rules for content filtering in the firewall
    - Provides both stateful and stateless packet filtering
    - Allows support for NAT and NAPT

# 13. What are the major tables in IPtables?

- `nat`: This is the table that does the NAT translation for addresses

- `filter`: This is the table that decides if the packet should be dropped or if it is able to reach its destination
    - This is the most widely-used table

- `mangle`: This is the table that allows packets to be changed, such as their headers. In case the machine that is reached is a router/switch it will decrease the TTL so that the packet may continue

![](https://docs.cumulusnetworks.com/images/old_doc_images/Linux-IPtables-Default-Tables.png)

# 14. What are `iptables` chains?

- Since `iptables` create rules there are 5 main conditions to be met by EVERY packet that crosses the firewall
    - `PREROUTE`: All built & ready packets enter this stage before they are routed
    - `INPUT`: Before the packet is "delivered" it needs to be sent to the firewall first
    - `FORWARD`: This chain is populated by packets that **ARE NOT** on the local network, i.e. those that need to be routed to another network
    - `OUTPUT`: All packets that are ready to be sent are populated in this chain
    - `POSTROUTE`: Packets have a routing destination set, they're out for delivery

![](https://www.booleanworld.com/wp-content/webp-express/webp-images/doc-root/wp-content/uploads/2017/06/Untitled-Diagram.png.webp)

# 15. How can you transparently use SOCKS (or other proxies) with IPtables?



# 16. What is STUN?

- _Knock unconscious or into a dazed or semiconscious state._ **See: Knock Out**
    - If some mf tries to diss you and ya bois you gotta STUN the mf

- The **_Session Traversal Utilities for NAT_** is widely used for interactive communications.
    - So, we have 2 hosts trying yo communicate with each other: **Alice** and **Bob**
        - **Alice** connects to the STUN server and says: `Hi! I'm Alice and I'm looking for Bob`
        - **Bob** does the same thing and connects to the STUN server and says `Hi! I'm Bob and I'm looking for Alice`
    - After this exchange, the STUN server _knows 2 things_ about each host: **_their public IP_** address and **_their port_**
        - **_Why the port?_** Since these 2 hosts are behind DNAT, the server has translated their ports

## 16.1 Why is it needed?

- It is needed to inform 2 parties/hosts about their location and ports for direct communication
    - The direct communication **_can only happen_** if both parties have `enpoint-independent filtering` setup

# 17. What are the three types of incoming packet filters? What are their filtering rules?

- **Endpoint-Independent Filtering**: Allows in _ANY_ incoming packet (regardless of precedence) to the private network as long as there exists a public --> private mapping
    - Allows anything if it exists

- **Address Dependent Filtering**: Allows packets from public internet connections, if and only if we (from our private network) our IP has previously contacted the public IP source on the internet
    - Allow connections `using addresses` only

- **Address and Port Dependent Filtering**: Allows packets from public internet connections, if and only if we (from our private network) have contacted the public IP source on the internet at a certain port
    - Allows connections using `address:port` only

![](https://raw.githubusercontent.com/aldogutierrez/NetworkManagementToolkit/master/pictures/3_nat_filters.png)

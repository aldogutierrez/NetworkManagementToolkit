# 1. What is the difference between the internet and and intranet?

- **Internet**: Otherwise known as the **WAN** (_Wide Area Network_) is a environment where routing takes place, this is where we make external connections, and frankly it is insercure for us.

- **Intranet**: Otherwise known as the LAN (Local Area Network) is the environment where we lie behind a firewall, this is where we make internal connections, and it is safer than the internet

![](https://i2.wp.com/networkinterview.com/wp-content/uploads/2019/03/img_5c7d45793170f.png?w=640&ssl=1)

# 2. Because big companies have their own intranets, how were they able to give back large blocks of reserved IPv4 addresses and starve off IPv4 address exhaustion?

- Because companies can have multiple intranets they just set-up **_NAT_** servers that won't need the same number of public IP addresses

# 3. What are the private IP network blocks?

- Private IP network blocks are commonly used for private networks and Local Area Networks, which use IPv4.

- IANA (Internet Assigned Numbers Authority) has assigned 3 main private address spaces
    - `10.0.0.0`
    - `172.16.0.0`
    - `192.168.0.0`

![](https://raw.githubusercontent.com/aldogutierrez/NetworkManagementToolkit/master/pictures/private_IPv4_address_spaces.png)

# 4. What is a walled garden?

- **_Walled gardens_** are a utopical representation of a perfecly safe network, we "_wall_" ourselves in a strong firewall to shield us from the dangerous content of the internet which we can use to safely browse in our intranet
    - Nowadays, viruses and bad configurations can expose and explose host vulnerabilities

# 5. Why is it a good idea to use secure channels like TLS even with applications that communicate inside of a walled garden?

- Transport Layer Security is used even inside walled gardens because it adds another layer of security

- In case that an intruder has breached the walled garden, using TLS will prevent attacks on communication

# 6. What is a DMZ?

- A **_Demilitarized Zone_** is a space/environment that is completely isolated from the intranet and the internet, it often lies between two networks or even within the network
    - Mail servers and Web server are often placed in a DMZ
- It is normal to have multiple services running inside a DMZ
- It is also normal to have multiple DMZs as well

![](https://upload.wikimedia.org/wikipedia/commons/thumb/6/60/DMZ_network_diagram_2_firewall.svg/1280px-DMZ_network_diagram_2_firewall.svg.png)

# 7. What is a bastion host?

- Bastion Hosts are machines/hosts that are connected to 2 networks; however, the networks are NOT connected to each other
    - These 2 networks cannot talk across the bastion host
    - Think about a mediator/man-in-the-middle
- Bastion Hosts rely a lot on isolation
- Bastion Hosts work a lot with `ssh`

![](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2017/11/15/NM_diagram_061316_a1.png)

# 8. Where is the firewall in relation to the internet, intranet, walled gardens, and DMZ?

- Firewalls are used to protect and filter out dangerous content from the internet towards an intranet

- In relation to the **_internet_**, there exists a firewall on the **_exit_** of an intranet
- In relation to the **_intranet_**, there exists a firewall on the **_entrance_** of the intranet
- In relation to a **_walled garden_**, there exists a firewall on the **_exit/entrance_** in order to communicate from the intranet to the internet
- In relation to a **_DMZ_**, there exists a firewall **_on both sides_** to where the services may be accessed

![](https://www.onlinecomputertips.com/images/networking/n159.jpg)

# 9. Why do we need isolated networks? (Why are we using the Pi's on an isolated network?)

- Isolated networks are used to protect other networks and itself, offering protections and more security.

# 10. What is the function of a firewall?

- A firewall is a protection barrier between a public and a private network. It protects the private network from any "unwanted" packets and connections from the internet

![](https://www.comodo.com/images/how-firewalls-work.png)

# 11. What is the difference between NAT, SOCKS, and application proxies?

- **NAT**: Allows a single device, normally a switch, to act as an agent between connections from the intranet to the internet. This agent has a public IP at the entrance, and a private IP at the exit, behind it, a private network that extends from the agent's subnet mask.
    - **NAT** runs on the _network layer_ (**L3**)

- **SOCKS**: Is an internet protocol that lies in the _transport later_ (**L4**), and exchanges packets using **TCP**.
    - There are _2 connections involved_:
        - The host establishes a connection to the SOCKS proxy on the firewall
        - The SOCKS proxy establishes an outgoing connection to the server in the internet
    - Once these 2 connections are established, the SOCKS proxy acts like a middleman for data transmission

- **Application Proxies**: These also act as a middleman to prevent direct requests/connections to the desired server
    - _HTTP Proxy_: We can connect to a HTTP proxy on the firewall, only works for HTTP requests, and run on the _application layer_ (**L7**)
        - The host passes the desired request to the proxy, which then makes the request from the firewall, **_ensuring anonymity_** for the host
            - These requests are done in **_2 hops_** 

## 11.1 Is there a reason to have all three? Or are there use cases for each technology?

- We generaly **do not** combine _NAT_, _SOCKS_, and _proxies_ together because each of these techonologies have their own use and may require different levels of transparency, which may interfere if combined

# 12. Why do we need VPNs?

- **_Virtual Private Networks_** are used to create an _encrypted tunnel_ to another network.
    - VPNs ensure an extra layer of security

- VPNs are needed to _access special information_ (school, enterprise) that may not be available in the hosts current domain.

- _VPNs connect from one intranet to another intranet_

# 13. How do the IP addresses you are using change when using a VPN?

- When using a VPN, while you may **_not be physically_** in the space of such intranet, **_you are there from a network standpoint_**. Your IP address will change since you are now part of another intranet
    - Your traffic will go through the VPN server, `which changes the IP`, and then to the destination; which the destination receives the request from the VPN server

# 14. Why would you want to use a VPN if you are connected to the internet? (at SBUX for example) and you are connecting to another internet service (like Netflix, for example)?

- In most cases internet connections from parks, airports, and Starbucks (for example) are LANs; however, malicious users may be capable of capturing packets from your connections, and compromise your security. This happens because **_it is easier to attack LANs than WANs_**

- Moreover, applications may log where you are connecting from, SBUX in this case, which can be avoided if we use a VPN

![](https://raw.githubusercontent.com/aldogutierrez/NetworkManagementToolkit/master/pictures/sbux_vpn.png)

# 15. What is the advantage of connecting remote offices together over VPNs rather than dedicated lines?

- It is **_cheaper_**, and allows for **_flexibility_** by adding bandwidth and switch servers/providers
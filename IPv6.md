# 1. How are IPv6 addresses written?

- IPv6 is the extension to IPv4 addresses
    - IPv6 are now `128-bits` long (4 times as long)

- IPv6 addresses are now written in HEX (more compact) and we use colons instead of dots

![](https://cdn.comparitech.com/wp-content/uploads/2019/01/IPV6-vs-IPV4.jpg)
## 1.1 What are some rules that are used to write them more compactly?

- Take the address `50B2:6400:0080:0000:6C3A:B17D:0000:10A9`

- There are 2 main rules that can be used to compact IPv6 representation
    - **No Leading Zeroes**: If one of the hextets has a couple of zeroes at the beginning we can omit them
        - Now our address becomes: `50B2:6400:80:0000:6C3A:B17D:0:10A9`
            - See how hextet #3 goes from `0080` to `80`
            - See how hextet #7 goes from `0000` to `0`
    - **Emptying an all-zero hextet**: IPv6 addresses can have all-zero hextets and these can be omited.
    - _NOTE_: We can ONLY OMIT ONE hextet, in case your IPv6 address has multiple all-zero hextets, you can only omit one and remove all leading zeroes
        - Now our address becomes: `50B2:6400:80::6C3A:B17D:0:10A9`

## 1.2 How do you write `host:port` form of host & port pairs when you are using IPv6 addresses for the host?

- Just like IPv4 (using the same addres as before), but we wrap the IPv6 address in square brackets ([])
- If we were connecting to port 6725 on address `50B2:6400:80::6C3A:B17D:0:10A9`
    - [50B2:6400:80::6C3A:B17D:0:10A9] : 6725

# 2. What are the 3 major types of IPv6 scopes?

- **Global**: Addresses on the internet (_WAN_)

- **Link-Local**: Only used for hosts on the same network (_LAN_)
    - Cannot connect to anything, cannot connect to a DHCP server, cannot connect to a router, etc...

- **Node-Local**: Only used in the same computer (_localhost_ for example)

## 2.1 What are they used for?

- IPv6 is known to use **multicasts** heavily; therefore, the IPv6 scopes help with defining the area topoly of the address' network
    - If the address belongs to the _global_ scope, it can **multicast** to ANYBODY on the internet
    - If the address belongs to the _link-local_ scope, it can **multicast** to ANYBODY on it's LAN
    - If the address belongs to the _node-local_ scope, it can **multicast** to itself

# 3. What do the following prefixes mean? 
- `::/0`: Denotes an all-zero IPv6 address, it is used to describe the default entry (also describes all IPv6 addresses)
    - There's no network, it is used when no specific address of a next-hop host is available, all the zeroes belong to the host

- `::/128`: Denotes a single all-zeroes address; however with comparison to `::/0` all the zeroes belong to the network
    - Can also be used to bind & listen for services
    - Also called the unspecified address

- `::1/128`: This is the IPv6 loopback address
    - Denotes an address where **_the whole thing belongs to the network_**, but the last bit is set to `1`

- `::FFFF:0:0/96`: This prefix is used as a mapper for IPv4 addresses
    - The **first 96 bits** belong to the network (The first 80 bits are 0's, and last 16 bits are 1's)
    - The **remaining 32 bits** belong to an IPv4 address

- `FE80::/10`: Denotes the **link-local addresses** for the computer
    - Only used to communicate with the LAN

- `FF00::/8`: Denotes multicast addresses, here only the first octet belongs to the network and its all-zeroes

![](https://raw.githubusercontent.com/aldogutierrez/NetworkManagementToolkit/master/pictures/ipv6_prefixes.png)

# 4. What are zone indexes?

- Zone indexes are used to specify which interface is being used for a repeated/reused IPv6 address.
    - These are used because in IPv6 you can have the same address for multiple interfaces

- These zones you can specify using a percentage sign after the IPv6 address
    - `FE80::1FF:FE23:4567:890A`**%**`eth2`
    - `FE80::1FF:FE23:4567:890A`**%**`3`

- Zone indexes are mostly used for link-local scoped addresses

# 5. How is multicast used in address to MAC resolution?

- IPv6 multicast works by separating the address into 2 parts:
    - Starts with `FF` at the beginning (8bits), then there are some flags attached after the `FF` (4bits), and finally the scope (4bits)
    - The remaining 112 bits are allocated for the **Group ID**

- **_Group IDs are bound to MAC addresses_** and they work in a _subscription manner_, you show interest into a multicast group. Then you MAC address will be part of the group and when there's a message sent, you will get the multicast message.
    - Servers that run applications using IPv6, may ask their users to "subscribe" to the application's multicast group

![](https://mrncciew.files.wordpress.com/2013/04/ipv6-06.png)

# 6. What is the name of the protocol that serves the purpose of ARP in IPv6?

- **Neighbor Discovering Protocol** (ND) is used instead of ARP for IPv6 to MAC address resolution
    - Makes use of ICMPv6
    - Makes use of multicast

- It works by multicasting from a multicast address to a link-local address
    - While multicast addresses can be routed to the internet, it is mostly used for link-local address resolution in the LAN
    - The multicast group is a very select group of machines that will listed to such messages, and it is based on the lower bits of your address

## 6.1 How does it use ICMPv6?

- ND uses 2 speficied ICMPv6 packets to work for MAC address resolution:
    - **Neighbor Solicitation** (_Type 152_): Requests address of multicast router
    - **Neighbor Advertise** (_Type 151_): Provides address of multicast router

See more at [RFC4286](https://tools.ietf.org/html/rfc4286)
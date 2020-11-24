# 1. List some advantages to having all of the machines at the site (such as a campus or office) on the same LAN.

- **Security**: Users can work on sentitive information without being seen vy other VLANs
- **Separation**: For broadcast messages
- **Easier Management**: We separate machines with software rather than with cables
- **Better Congestion Control**: Each VLAN does not have to deal with lots of traffic, reduces the need of routers

## 1.1 List some disadvantages.

- In order to have VLANs talk to each other, we need to set up a router in between
    - We undo the congestion control advantage
- Other hosts will need to be part of such VLAN to talk to one of the hosts, reduces interoperability
- Management becomes complex
- VLANs use more bandwith than a normal LAN

# 2. How do VLANs help preserve the advantages of having all machines on the same LAN, while mitigating some of the disadvantages?

- If all machines are on the same LAN we limit the entries and exits to the network as **_everyone will use the same router and switch._**
- This LAN would be separated and isolated, making the management easier, and more secure

![](https://wiki.mikrotik.com/images/9/9a/Image12005.gif)

# 3. Use `tcpdump` and Wireshark on your city's virtual machine to look at the Ethernet packets there.

## 3.1 Is the VLAN shown in the Ethernet frame?

- NO

## 3.2 How do you think the VLANs work on the switch your city is on?

- VLANs are all about switching, so yes...

# 4. How can you know of an Ethernet frame has a VLAN field or not? In other words, how do you know whether the bytes following the lenght/type field are data or the VLANs tag?

- The bytes following the length/type must be `0x8100` in order to mark a `801.1Q` header, which symbolizes the VLAN

# 5. In what a scenario must the VLAN tag (802.1 p/q) be present?

- The tag `802.1 p/q` must be present when the switch receives a packet that needs to be delivered to a host on the same VLAN, the tag would tell the switch that it does not need to forward it to a router, but rather just identify the destination and switch the packet

# 6. Can a server be on multiple VLANs using a single Ethernet NIC?

- Yes

## 6.1 If so, how? If not, why?

- The servers must open the the port that NIC is listening to, and setup a cofiguration to accept ethernet packets with `802.1` included in their header
    - The `802.1` tag on the Ethernet packet helps identify the VLAN that it belongs to
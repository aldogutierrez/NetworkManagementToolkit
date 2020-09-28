# 1. What is ARP used for?

- The Address Resolution Protocol (ARP) is used to look-up a MAC address from an IP address

- **GOAL**: To discover link layer addresses from a given network address

# 2. What layer does ARP run at?

- ARP is part of the **data-link layer** (_layer #2_)

# 3. How is ARP used when sending IP packets to destinations not on the local network?

1. First, we bradcast an Ethernet frame from our MAC address to **FF:FF:FF:FF:FF:FF**
    - This broadcast is accepted by all computers in the local network

2. The correspondent computer received the broadcast and answers with its MAC address

3. We then cache the MAC address to our ARP cache for later use

# 4. Why are some ARP messages broadcast and other not?

- **Request**: `Broadcasted`, because the sender doesn't know which MAC to send to.

- **Reply**: `Unicasted`, because the sender does know the MAC address to send to.

# 5. Why do we need an ARP cache?

- We need the **ARP cache** in case we need to communicate with the same host again, that way we don't need to send another ARP broadcast

# 6. How is the ARP cache kept up-to-date?

- In case we **dynamically** use ARP, each entry expires after 20 minutes since the last use.

- Every time we use the cached ARP value, we reset the timeout
    - This is an example of `soft state`, meaning that we must refresh the addresses to avoid expiration

# 7. What happens if you try to use ARP to find an address that does not exist?

- If the host we are trying to communicate with is unreachable, we just re-transmit the ARP broadcast
    - If we get no response, we get an incomplete cache (the ARP cache will expire after 3 minutes)

# 8. What is a gratuitous ARP?

- We ARP ourselves, to update everyone else's MAC address. That way, everyone that has ARPed and cached our MAC address, updates theirs

- Everytime we use ARP and get a successfull response, two ARP caches get updated, **ours** and our **target**

- It works just like broadcasting your identity so that everyone that has cached your MAC, can update their table

# 9. What is ARP poisoning?

- ARP poisoning is a type of **Man-In-The-Middle attack**

- First, you mask yourself as the router (in the eyes of the victim host) and ask them to store your MAC address
- Then, you mask yourself as the host (in the eyes of the router) and ask the router to cache your **fake** MAC address
    - Eventually, all their traffic gets forwarded to you

![](https://www.imperva.com/learn/wp-content/uploads/sites/13/2020/03/thumbnail_he-ARP-spoofing-attacker-pretends-to-be-both-sides-of-a-network-communication-channel.jpg)

# 10. How does the receiver of an ARP response know that it got valid data?

- ARP is an extremely insecure protocol, and in reality, we are never really sure that the response we get belongs to the desired host


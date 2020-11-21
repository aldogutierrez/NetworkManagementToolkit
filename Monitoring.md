# 1. What layer sends and receives the ICMP packets?

- **Sending (source) layer**: Network
- **Receiving (destination) layer**: Transport

![](https://www.ictshore.com/wp-content/uploads/2017/01/qne001-01-ICMP_packet.png)

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

# 4. How would you see the connections on a Linux machine?

- We make use of `netstat -an`

![](https://2.bp.blogspot.com/-lZb0NQ90Gxk/Wq_sJ8eF7HI/AAAAAAAAGBM/oeRYQNaXPeoeVVlo2BexGwiHMnA9JtoHQCLcBGAs/s1600/netstat%2B-n.png)

# 5. How do you see the network configuration of a Linux machine?

- `nmtui`

# 6. What commands can you use to setup network interfaces?

- `nmtui`
- `ifconfig`

# 7. What does the `ss -i` command tell you about a TCP connection?

- `ss` is a tool that dumps `TCP` information
    - It is very similar to `netstat`
- It can display more TCP and state information than other tools.
    - According to the `man` page

- `ss` uses the `-i` flag to dump `TCP` connection-related data, it includes, rto, rtt, rco, etc.

- It seems like `ss -i` extracts the value from the `TCP` header and dumps them on the screen, as the majority of the values belong to the header

![](https://raw.githubusercontent.com/aldogutierrez/NetworkManagementToolkit/master/pictures/ss_i.png)

## 7.1 Why is it useful?

- It helps with getting to know the connection better
- We get useful information about the state of the connection
- We get internal information from `TCP`

# 8. What the broadcast address for `18.3.4.6/20`?

- **REMEMBER**: Broadcast address = (Network Address) && (Flood the rest with 1's)
- Therefore if the network address is `18.3.0.0` & we flood the rest with 1's
- `18.3.15.255`

## 8.1 What is the subnet mask?

- For the subnet mask, we substitute with 1's the number of the CIDR notation
    - Therefore, the first 20 bits
- `255.255.240.0`

## 8.2 What is the network address?

- The network address is the 20 first bits of `18.3.4.6/20`
    - `18.3` are part of the network address (first 16 bits)
    - The rest of the network address is the first 4 bits of `4`
        - `4` in binary is `00000100`
- Therefore the network address is `18.3.0.0`

# 9. How would you write the code to answer #8?



# 10. Given the routing table:

```
$ netstat -r

Kernel IP routing table

Destination         Gateway         Genmask             Flags       MSS         Window      irtt        Iface
default             gateway         0.0.0.0             UG          0           0           0           eth0 
10.32.41.0          0.0.0.0         255.255.255.0       U           0           0           0           eth0
link-local          0.0.0.0         255.255.0.0         U           0           0           0           eth0
link-local          0.0.0.0         255.255.0.0         U           0           0           0           eth1
172.30.2.0          0.0.0.0         255.255.255.0       U           0           0           0           eth1
192.168.122.0       0.0.0.0         255.255.255.0       U           0           0           0           virbr0
```

## 10.1 Which interface would be used to access 8.8.8.8?

- We have 3 main interfaces `eth0`, `eth1`, and `virbr0`
    - `eth0` only has the IP address `10.32.41.0` apart from `default` and `link-local`
        - Network address: `10.32.41.0`
        - Subnet mask: `255.255.255.0`
            - First usable address: `10.32.41.0`
            - Last usable address: `10.32.41.255`
        - Subnet mask: `255.255.0.0` **(?)**
            - First usable address: `10.32.0.0`
            - Last usable address: `10.32.255.255`
    - `eth1` only has the IP address `172.30.2.0` apart from `link-local`
        - Network address: `172.30.2.0`
        - Subnet mask: `255.255.255.0`
            - First usable address: `172.30.2.0`
            - Last usable address: `172.30.2.255`
        - Subnet mask: `255.255.0.0` **(?)**
            - First usable address: `172.30.0.0`
            - Last usable address: `172.30.255.255`
    - `virbr0` only has the IP address `192.168.122.0`
        - Network address: `192.168.122.0`
        - Subnet mask: `255.255.255.0`
            - First usable address: `192.168.122.0`
            - Last usable address: `192.168.122.255`

If we take IP `8.8.8.8`, we don't know its `network` nor its `subnet mask` but it is not bounded to any of the 3 interfaces as all interfaces have their 3 first octects bounded to `10.32.41.x`, `172.30.2.x` or `192.168.122.x`; therefore, not granting enough space for `8.8.8.8`

- **None**, these interfaces do not reside in `8.8.8.8`'s network space

## 10.2 What about 192.168.122.4?

Based on the information from `10.1`, the only interface that is able to accomodate `192.168.122.4` is `192.168.122.0`. This is because the last octet has an opened range for any host with `0-255`

- Then, `virbr0` is the only interface that can be used to access `192.168.122.4`

## 10.3 What about 192.168.120.4?

The only interface that resembles `192.168.120.4` is `virbr0`; however, we see that the 3rd octed may impose a problem.

- In binary `192.168.122.0` is `11000000.10101000.01111010.00000000`
- And `192.168.120.4` is `11000000.10101000.01111000.00000100`


- Doing bitwise `AND` we get `192.168.120.0`
    - By doing bitwise `AND` we can accomodate `192.168.120.4` into `virbr0`

- Otherwise, **_none of the iterfaces_** can accomodate the address

# 11. Why is freshness of monitoring data important?

- If the data is fresh, then the systems are polled continuously and we get as close as "*real-time*" we can get
- It is important because if we were to use "*over-time*" data, the systems are polled every 6-24 hours, and if something happened in betweem we wouldn't know

# 12. Why do you need long-term data?

- The advantage of long-term data over short-term data is that we are able to see the greater spectrum of the system, in case there is a problem, we would be able to see where the error propagates, and pinpoint where the problem originated.
- With short-term data, it is common to receive alerts that are not urgent, and the system just had a sort of "hiccup"

# 13. Why isn't average sufficient for statistical functions?

- Taking averages may lead to biased estimations, because averages are pulled towards the direction of the outliers
- Not all users may have the "**_average_**" experience

# 14. Why do you need categories of alerts?

- It is good to know the _tier_ of the alert to know about the **serverity**

# 15. What is alert suppression

- Alert suppression is a mechanism in which we are able to filter _false posives_ and _false negatives_ from the alerts

## 14.1 Why do you need it?

- Dictates all the false positives and negatives from the alerts
- Makes users realize that alerts need to be fixed and in a _functional manner_, avoiding the _quick-and-dirty_ approach

# 16. What are 4 example sources of monitoring data?

- CPU Usage
- RAM Usage
- Disk Usage
- Temperature

# 17. Why do you need to treat your configuration data as code?

- **Version Control**: You add a configuration that does not really work, by having a VCS you can revert back to the config that actually worked
- **Testing**: You add configs to speed up your application, you can test for 1 week on some configs and compare performance
- **Libraries as Configurations**: Some libraries are heavy, unless really needed, you should not add the whole framework for an application
- **Loose Coupling**: **_See below_**

# 18. What does loose coupling refer to?

- Loose coupling refers to the coordination and communication of modules, a loosely-coupled system has more modules and each modules does a VERY specific task, so that if problems arise they are easier to debug, easier to test, and easier to develop.
- Loose coupling adds flexibility, and reusability

![](https://media.geeksforgeeks.org/wp-content/uploads/Untitled-28.png)

## 18.1 Is there a downside to loose coupling?

- **Inconsistency**: If we build a loosely-coupled system, as the system grows, we might have issues maintaining data consistency, as these modules now communicate differently and exchange/modify data separately.

In the above picture, imagine each node to be `A`, `B`, `C`, and `D`, these nodes are connected as they communicate with each other but we can begin the process at any on these nodes.

- Say we begin with `A`, `A` can then go to `B` or `D`, depending on which module is chosen we will get the same result; however, with a _different_ structure

# 19. What is the problem with monitoring everything?

- Monitoring data is stored in logs, these logs cost money to maintain and back-up, if we were to monitor everything, each polling message will add cost and latency
    - Frequent measurements may be very _expensive to collect_, _store_, and _analyze_

- Say we have a server with `99.99%` uptime throughout 12 months, polling it every 5 seconds just adds unnecesary messaging
    - Polling constantly is a bad example of **_random granularity_**

## 19.1 How do you choose what to monitor?

- We are mostly interested in monitoring modules that have a greater responsibility in the system
- We also monitor parts that are prone to failures
- The rules that catch real incidents most often should be as **_simple_** (monitoring a few things), **_predictable_** (based on long-term analisys/data), and **_reliable_** (should be up 24/7) as possible.

# 20. How do you test monitoring?

- We could look at components from its surface (_black-box_) or internally (_white-box_)

**NOTE**: A good monitoring setup is able to **_detect problems early_** and alert employees to **_react appropriately_**

# 21. How do you figure out when to alert?

There are 2 scenarios where alerting becomes useful:

- Something is broken, and somebody needs to fix it right now!
    - Server is no longer turned on
- Or, something might break soon, so somebody should look soon.
    - CPU usage is up 95%, and will become saturated any moment

# 22. What is the difference between black-box and white-box testing?

- **Black-Box Testing**: High-level, on the surface testing, we do not pay atttention to the internal flow of functionality, and in monitoring the component becomes externally visible, its behavior as a user would see it.

- **White-Box Testing**: We become more interested on the internal flow of functionality, we check modules, dependencies, states, and complexity; including logs, interfaces like the **_Java Virtual Machine Profiling Interface_**, or an **_HTTP handler_** that emits internal statistics.

**NOTE**: _We are more interested in white-box testing/monitoring_

# 23. What are the "four golden signals"?
They all go hand-in-hand:
- Latency: _How long is it taking?_
- Errors:
- Traffic:
- Saturation: _Did we run out of space/memory?_

# 1. What is load balancing?

- Is a set of strategies that facilitate scaling up or scaling out when a server or service becomes overwhelmed
    - Its goal is to evenly distribute load across servers

![](https://miro.medium.com/max/668/1*Mgrhryt4agUB2CMiGjyJNQ.png)

# 2. Sometimes we can load balance by spreading each request to the servers providing the service, but sometimes we want to have a a client persistently use the server that we routed its first request to. Why?

- Say you have an Instagram account, and when you created your account your request was processed by a server in **_San Francisco_**, and you have created some activity, followed some people, liked some pictures. Now, most, if not all, of your account data is stored in the SF server.
- Now you're visiting a friend in Los Angeles, and you want to connect to Instagram, and you resolve to a couple of DNS servers of nearby Instagram servers in LA.
    - You can connect to those LA servers, **but** ALL the data from the SF server needs to be routed to the server which you are connected to.
        - You have now essentally created more traffic for the SF server (sending everything to LA) and the LA server (delivering the content to you)

- By routing users requests to the server where they first routed their first request we keep them where their data is and we won't have to involve other servers to store/cache/route the traffic

# 3. How can we do application layer load balancing?

- If users want to request **HTTP** pages, there are HTTP response messages in the heade
    - **404** (4XX _Client Errors_): Page Not Found
    - **500** (5XX _Server Errors_): Internal Server Error
    - **200** (2XX _Success_): OK
    - **300** (3XX _Redirection_): Client must take additional action to complete the request
        - **301**: Moved Permanently
            - This and all future requests should be directed to the given URL

- Since HTTP runs on the application layer, the HTTP messaging system alerts the user when pages have been found, when there are errors, and when they need to go to another URL
    - **_The HTTP messages work as a load balancer for requests_**

# 4. How can we do load balancing with DNS?

- DNS **_can_** resolve for servers **_based on location_**
    - On the DNS **header**, we can **modify** it to give some information about the available servers

- However, **_if we rely on DNS_** servers to take care of load balancing, these **_servers can become overloaded_**
    - **PROBLEM**: users would need to query twice and invalidate the previous cache entry

- While DNS can return many other resolved IPs, **_DNS cannot really recommend_** a "free and active" server

- We can also **_set a lower TTL_** for cache entries, but traffic will increase as queries will need to be ran again and again

# 5. How can we do load balancing with VIP and NAT?

- VIP (Virtual IP) **_is just a router_**, that is placed in front of the servers
    - To the user it will look just like a server

- **_VIP works just like DNAT_**, it changes the user packets so that they reach an available server.
    - After the DNAT has been taken care of, the ACTUAL server can communicate directly with the user, without the need of passing through the VIP
        - This can only work if and only if, the server knows it's behind the VIP

![](https://raw.githubusercontent.com/aldogutierrez/NetworkManagementToolkit/master/pictures/vip_load_balancing.png)

## 6. Find scenarios for each of the 3 above methods in which it is the best method to do load balancing.

- **Application**: A webpage has been moved from one server to another, HTTP just gives you another URL to where you can get such page

- **DNS**: If there are multiple DNS responses to a query, we can just chose a server to connect to using the round-robin algorithm, which effectively distributes the load across the server group.

- **VIP + NAT**: It is possible that to access a service, we only resolve to 1 IP address, it is highly probable that this address is a VIP, which then redirects our traffic to an actual server behind the VIP
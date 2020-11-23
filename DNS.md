# 1. What is the difference between gTLDs and ccTLDs?

- `gTLDs` (Generic Top-Level Domains): Are the most **_well known and general_**, they include `.com`, `.net`, `.gov`, etc...

- `ccTLDs` (country code Top-Level Domains): These codes **_are assigned to countries_** to host their own domains, these include `.de`, `.fr`, `.mx`, `.in` etc...

- Oftentimes, these domains may collide, if a television company wants to set up webpage, they're most likely to use `.tv` domain; however, this belongs to the country of _Tevalu_

![](https://raw.githubusercontent.com/aldogutierrez/NetworkManagementToolkit/master/pictures/dns_structure.png)

# 2. Why do most TLDs not allow all unique characters?

- In TLDs structure, unique characters can be replaced by another encoding called **punnycode**

- IDN TLDs (domains that include unique characters) are no longer a thing, **_because it is possible to spoof_** a URL by using encoding that may look normal to a user
    - "`bücher.de`" -> "`xn--bcher-kva.de`"

# 3. What is a DNS zone?

- The DNS is structured is that it is split into tiers, because a single entity managing and overseeing the whole DNS directory would be a disaster.

- Some tiers overlook and manage `.com`, `.net`, and `.edu` such as **Verisign**

![](https://ns1.com/writable/images/domain-zones@UI.png)

## 3.1 Why are they important for scalable management?

- Instead of having a singular base for these TLDs, we can add another server that can manage these requests faster

# 4. Why are there at least 2 managing servers required for a DNS zone?

- The purpose that there are 2 servers up & resolving DNS queries is to provide their services more efficiently
    - If there was only 1 server, and it goes down, users cannot longer resolve and cache the queries

# 5. Where is caching used in DNS?

- **_Caching is done in the very same DNS server_**, and it is used so that if there is another query to such domain, it can resolve it faster without the need to redo que query
    - Of course these caching entries will eventually become invalidated

- DNS uses two techniques: _level of indirection_ (referencing/dereferencing) & _caching_

- The only DNS servers that cache results are **_recursive_**
    - When a result is cached it also includes a TTL, symbolizing the caching time for the query

# 6. What is negative caching?

- There are times that users want to access a **_URL that does not exist_**, in this case the DNS server will not resolve the query and throw and error

- The DNS server will then cache this **_error_** so that it notifies the user that such domain does not exist
    - This is called negative caching

# 7. What is an authoritative server?

- Authoritative servers are DNS servers that **_include endless pointers_** and **_some answer_** to other DNS servers that may resolve the query

- The way they work is:
    - If a query is processed by the authoritative server, and _if it knows the answer_, it will give it to you
    - If this server _does not know_, it will pass the query to another server that may have the answer

# 8. Why is TTL used in DNS?

- The **_Time-To-Live_** field is used in DNS caching. Once a query is resolved, there's a TTL attached to it, meaning the time that it can stay in the DNS's cache

# 9. How does DNS name to address resolution work?

- Because users won't memorize IP addresses for their domain, they will create a URL, this URL will then get translated into an IP address where the service can be accessed.

- The kind of servers that will always get us an answer are **_recursive servers_**, these servers will ask around until they get an answer, or a "non-existent" message

![](https://raw.githubusercontent.com/aldogutierrez/NetworkManagementToolkit/master/pictures/dns_query.png)

# 10. What is the `in-addr.arpa` domain used for?

- The `in-addr.arpa` domain is used to **_resolve reverse DNS queries_**
    - That means **from IP to URL**

- If you would like to resolve `8.8.8.8` to the URL format, you would attach `in-addr.arpa` at the end and send the query
    - `8.8.8.8.in-addr.arpa`

- When resolved, the query will return `google-public-dns-b.google.com`
    - We can also re-confirm by doing a regular DNS query for `google-public-dns-b.google.com`, if it were to return `8.8.8.8` we would call this **_forward-confirmed_**

![](https://images.ctfassets.net/plii0v5gbc4s/1Y1Z04UYBC4OIVcUYb2MIl/52f06f2598e7e7024a2a21957b12390a/reverse-dns-example.png)

# 11. What can DNS do besides resolve names to IP addresses (table 11-3)

- DNS, apart from resolving names to IP addresses, can also resolve **_PTR_** (for reverse DNS queries), **_MX_** (mail exchangers), **_TXT_** (you can even send text), and **_SOA_** (Start Of Authority, used to get info about the DNS zone you're in)

![](https://raw.githubusercontent.com/aldogutierrez/NetworkManagementToolkit/master/pictures/dns_capabilities.png)

# 12. Why does DNS support both TCP and UDP?

- DNS runs over the _application layer_ (**L7**)

- **_UDP_**: Normally, DNS queries/responses are delivered using UDP, because its limit size is 512 bytes

![](https://raw.githubusercontent.com/aldogutierrez/NetworkManagementToolkit/master/pictures/dns_udp.png)

- **_TCP_**: In case your query/response is greater than 512 bytes, TCP is encouraged as it creates a bytestream for the question/answer

# 13. Why is there a transaction ID in the DNS header?

- The 16-bit transaction ID is used to track questions to responses.
    - Each DNS packet starts with a transaction ID, the same query & answer carry the same ID
    - Ensures that it gets to where it belongs

![](https://raw.githubusercontent.com/aldogutierrez/NetworkManagementToolkit/master/pictures/dns_fixed_header.png)

# 14. Why would a name resolve to multiple IP addresses?

- It is common that server do not have a single point of service, and they may have multiple

- If a name resolves to multiple IP addresses, these are just simply **_other servers that we can ask or connect to_**

- In class **_Yahoo!_** responded with 6 IP addresses
    - How do we chose the one that is closes? Better? Faster? _We don't know_; there is no algorithm that dictates the IP address priority for DNS queries

# 15. Look at the list of nameservers on your machine.

![](https://raw.githubusercontent.com/aldogutierrez/NetworkManagementToolkit/master/pictures/dns_nameservers.png)

## 15.1 Why do you think there are more than 2?

- For redundancy purposes, if one were to fail, our requests can utilize the other server
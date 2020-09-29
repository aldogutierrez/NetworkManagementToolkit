# SIMPLE NETWORK MANAGEMENT PROTOCOL (simple, but not very secure)

# 1. What was the goal of SNMP? (RFC 1157)

- According to `RFC 1157` SNMP intended to minimize the number and complexity of management functions realized by the management agent.

- Let network administrators not only manage networks, but manage both networks and applications

- SNMP manages devices using `GET` and `SET` requests and uses `notifications <traps> and <notifications>`

# 2. Did it succeed?

- Nope, it was intended to be the only management protocol, but it had a lot of security issues

# 3. How are SMI (RFC 1155), MIB (RFC 1214), and SNMP (RFC 1157) related?

- `RCF 1155`: Describes the data structures used (SMI)
    - The data structure used for management looks like:
        -   directory     OBJECT IDENTIFIER ::= { internet 1 }
            mgmt          OBJECT IDENTIFIER ::= { internet 2 }
            experimental  OBJECT IDENTIFIER ::= { internet 3 }
            private       OBJECT IDENTIFIER ::= { internet 4 }
        - If we want to manage document #1, we can traverse the tree until we find it.
        - All `object identifiers` start with `1.3.6.1`
            - So `{mgmt 1} = {1.3.6.1.2.1}`

- `RFC 1214 {Management Information Base}`: Describes the Structure of the MIB for both agent and client
    - Added generation for asynchonous events

- `RFC 1157 {SNMP}`: Describes the SNMP protocol itself

These 3 RFCs are related like so:

SNMP {`RFC 1157`} <uses> -> MIB {`RFC 1214`} -> which uses -> SMI {`RFC 1155`} to describe the data stored in the MIB

# 4. What is an OID?

- The `OID` is the **object identifier** and it is assigned to uniquely identify the partion of the network to manage

- It is assigned by the administrator

# 5. How are OIDs used to allow MIBs that work accross devices as well as allow vendors to create their own MIBs?

- OID's are used for the manager to control the agents {`agents = devices`}, the OIDs are stored in the MIB but described in the SMI

- `**BOTH**` managers and agents must agree on using the same MIB structure, otherwise the tree traversal may not match

- All devices must use the same MIB {but it is possible that different devices have varying MIB fields}

# 6. How is SMI related to ASN.1?

- Just like `JSON` uses JavaScript objects to describe the data, `ASN.1` uses `SMI` to describe the data

# 7. How is ASN.1 related to SNMP? (there are two general uses)

- SNMP uses the `ASN.1` protocol to send data among its agents

- `ASN.1` was surpassed by `protobufs`

- `ASN.1` belongs to the same family as `JSON` and `XML`

# 8. What are the operations available in SNMP v1?

- `get-request`: Asks the agent for status on any flag requested, it contains the names of the MIB objects to be retrieved
- `get-next-request`: Asks for the next variable in the MIB from the initial request
- `set-request`: The manager can change values for the agent's MIB
- `get-response`: The manager processes the response sent by the agent
- `trap`: Requests an update message on any MIB fields

# 9. If the only operations to change state is SetRequest which changes the value of a variable.
## How are important/crucial operations, such as reboot, available in SNMP v1?

- To reboot, we have to turn the device off, then turn it on
    - We would issue a `SetRequest` for the devices status from `ON` to `OFF`
    - We would issue a `trap` message to update us when the device is turned off
    - Finally, we would issue another `SetRequest` for the devices status from `OFF` to `ON`

# 10. Look at the MIB-2 mib (/usr/share/snmp/mibs/RFC1213-MIB.txt on your group server)
##  a. Use the MIB to figure out the numeric OID for sysUpTime

- The MIB says `sysUpTime` has a value of  `system 3`

##  b. What does the OID 1.3.6.1.2.1.4.18.0 represent?

- This presents the IP state of the device, which has value `IP-MIB::ipFragFails.0 = Counter32: 0`

# 11. What are SNMP traps used for?

- The manager can request a `trap` message that `notifies` the manager about any changes in the MIB

## 11.1 What is the problem with using them?

- The problem with `traps` is that since **ALL** of SNMP runs over UDP, _trap notifications can get lost_
    - If a `trap` is sent and then **lost** _NOBODY WILL KNOW_
    - Sender {agent} hopes it arrives, but the receiver won't even know its coming

## 11.2 How does inform help?

- In the case of `inform` the sender expects a response
    - The manager will **ACK** the `inform` message

- If the response is **NOT** received, the sender {agent} will keep resending
    - However, the sender cannot re-send forever [there is a `cutoff = 6` in the class' example]

# 12. What improvements did SNMP v2c bring?

- Adds support for 64-bit counters

- SNMP v2c added `informs` as a more reliable replacement for `traps`

![](https://www.tp-link.com/us/configuration-guides/configuring_snmp_rmon/configuring_snmp_rmon-web-resources/image/table1-1.png)

# 13. What improvements did SNMP v3 bring?

- SNMP v3 added **user authentication** and **encription security** such as `MD5`, `DES`, and `AES`
    - Authentication is based on regular username/password as it no longer uses the community name `public` as the password

- Just like SNMP v2c, it still uses `inform` rather than  `trap`


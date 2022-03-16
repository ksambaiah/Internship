## DNS
We know that DNS is an application protocol, that uses udp (one rare case tcp we cover later) protocol and port is 53. 

### You need client

Your browser is doing work for translation of the name to address or ping or other commands you don't need to do dns resolution other than finding out some times. 
Your dns is provided by DHCP server (most of the time your router) and updates the file `/etc/resolv.conf` for Linux and Mac systems and ipconfig will tell Windows DNS servers.

```
nslookup
> www.purdue.edu
Server:		72.163.128.140
Address:	72.163.128.140#53

Non-authoritative answer:
Name:	www.purdue.edu
Address: 128.210.7.200
> set type=ns
> purdue.edu
Server:		64.104.128.236
Address:	64.104.128.236#53

Non-authoritative answer:
purdue.edu	nameserver = pendragon.cs.purdue.edu.
purdue.edu	nameserver = ns3.purdue.edu.
purdue.edu	nameserver = ns4.purdue.edu.
purdue.edu	nameserver = ns1.rice.edu.
purdue.edu	nameserver = ns2.rice.edu.
purdue.edu	nameserver = harbor.ecn.purdue.edu.

Authoritative answers can be found from:
ns1.rice.edu	internet address = 128.42.209.32
ns3.purdue.edu	internet address = 128.210.224.226
> set type=PTR
> 72.163.128.140
Server:		64.104.128.236
Address:	64.104.128.236#53

$cat /etc/resolv.conf
search cisco.com
nameserver 64.104.128.236
nameserver 72.163.128.140
```
Let us try to understand the above output.

We wanted to know the address of www.purdue.edu and got as 128.210.7.200 request went to get this went to 72.163.128.140
we wanted to know who will provide me an answer to   purdue.edu (we also called nameserver so set type=ns) and got as 128.42.209.32 and 128.210.224.226
PTR record is not important at this point
We are getting answers from 72.163.128.140 and 64.104.128.236. Why are these two servers providing answers?
/etc/resolv.conf file is updated by DHCP (even VPN, that is other time) has data search cisco.com nameserver 64.104.128.236 nameserver 72.163.128.140
Any query for DNS reaches out to the two servers listed above
Notice is that DNS is a critical infrastructure on the Internet. If DNS is down if service is up, no way we use service.
October 2021 Facebook is down because DNS servers are failed.
DNS is fairly distributed very rare to fail.
We understood queries and we don't do unless we wanted to debug. We would like to know how to set up DNS and what kind of servers are available.

If you wanted to get resolved and don't have any setup you call it resolver only configuration.
If you have some data for a domain to publish you need to configure primary and secondary name servers. Any modifications on the primary server will be populated to the secondary server
Historically Bind was the most popular software (though difficult to say it is resolver only or primary/seconday to domain), DJBDNS and AWS Route53 etc.
We covered a small part of DNS, other topics we cover later.


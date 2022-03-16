## Private IP and NAT
We covered the Application layer and TCP/UDP layer. Application layer roughly refers to providing service to user like http or ssh or DNS. TCP layer covers the transport of packets and reliability of packets. IP layer for routing and addressing packets of data so that they can travel across networks and arrive at the correct destination. Data traversing the Internet is divided into smaller pieces, called packets. IP information is attached to each packet, and this information helps routers to send packets to the right place. Every device or domain that connects to the Internet is assigned an IP address, and as packets are directed to the IP address attached to them, data arrives where it is needed.

Once the packets arrive at their destination, they are handled differently depending on which transport protocol is used in combination with IP. The most common transport protocols are TCP and UDP.

Application layer ---> TCP or UDP ---> IP 
This suite of protocol we refer as TCP/IP.

IP layer consists of the transfer of packets, it comes under IP address space. The version widely used is 4 so it is called ipv4.

Any device that needs to communicate with another one using TCP requires an  IP address.
Current version is IPv4
IPv6 is rapidly coming, but it takes some time to adopt.

IPv4 address is a 32 bit address space, each 8 bits are separated by a dot so the address looks like 172.16.100.12. A flat network with address 10.2.3.4 communication with 192.16.14.20 requires understanding 2^32 locations is an impossible task. 32 bit address space is split into two parts, one part is the network part.

172.16.100.0/24 network means the first 24 bits is for network and you can have IP range from 172.16.100.0 to 172.16.100.255. Any subnet part all zero and all one are reserved, valid range is 172.16.100.1 to 172.16.100.254.
 172.16.100.24/32 means it is a single IP address
10.0.0.0/8 have 2^{18}-2 addresses
Organisations will have thousands of computers, mobile devices and home computers combined to cross the limit of 2^{32} if we start giving IP  for each device. How do we solve the problem of not having enough addresses? 
Idea 1. Most of the time home or organization systems communicate with each other and rarely do they need to talk to the outside world.
Communication within the organization call private and outside public. 
Specify a range of addresses for private and no one in the public get this range and ignored if seen in the public network.
[RFC 1918](https://datatracker.ietf.org/doc/html/rfc1918) Covers details of private address space.
RFC 1918 defines the following address ranges as private
> 10.0.0.0/8 (addresses 10.0.0.0 through 10.255.255.255 inclusive)
> 172.16.0.0/12 (addresses 172.16.0.0 through 172.31.255.255 inclusive)
> 192.168.0.0/16 (addresses 192.168.0.0 through 192.168.255.255 inclusive)
> The addresses in these ranges are not routable in the Internet and may be freely used by any organisation for local purposes only.

Each system or mobile devie required to browse needs to have IP address
Private address space also called RFC1918 address space will not be able to communicate to internet.
Private address space is also called RFC1918
Routable address is called public address
If you have multiple devices at home, each one gets private IP from the router and the router gets public IP from the Internet provider. We will cover how your private IP is routed to the internet from the router.


## We will cover ports, 5 tuple, and NAT

## Port numbers
We covered in the previous chapter about applications and TCP layer.  HTTP is an application layer and it uses TCP (other one being UDP) for transport and port number 80. 
You fire a browser and visit www.google.com site, what happens?
Your IP, www.google.com, 80, TCP this four tuple is involved in initiating communication. If only four tuple are used, you can't open another session with www.google.com. There will be a dynamic number between 1024 to 65355 and one not used (we come to this later) make 5 tuple.

(Your IP, Dynamic port, www.google.com, 80, tcp) we refer to as 5 tuple. 

A sample of 5 tuple looks like
(129.16.17.231, 23456, 142.250.192.68, 443, tcp) 

## NAT Network address translation

We discussed private IP space (also called RFC1918 addresses) and your home computer/mobile gets it from router via DHCP protocol. A sample 5 tuple looks like
(192.168.29.40, 23745, 142.250.192.68, 443, tcp) 
We know that 192.168.29.40 is not routable to the internet, how is it able to communicate?

Router works like Network address translation or NAT device (You will learn later it is called SNAT source network address translation)
Router has public IP assigned from Internet Service provider
Magic happens (192.168.29.40, 23745, 142.250.192.68, 443, tcp)  is sent to the internet as (PublicIPofRouter, 34821, 142.250.192.68, 443, tcp)
Router keeps table of 192.168.29.40, 23745 --> PublicIP, 34821
Once data received to public IP port number 23745, it will be replaced with 192.168.29.40
Source or originator network address and port is translated so it is called Source NAT


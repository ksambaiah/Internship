# Networking 101
## Basics of Networking are covered in this short paper.

## IP address

- How do your system get IP address?
- Apart from IP address what else information your system gets?

Home router runs a protocol called DHCP Dynamic Host Configuration protocol service. If you connect your computer by cable then it contacts the router and router allocates IP using DHCP protocol. If you connect via WIFI same mechanishm works and your computer gets IP address.

> Linux you can find ip address using command $ip a
> Windows ipconfig command gives ip address.

If you get ip address as 192.168.29.24 and netmask as 255.255.255.0 
What is the meaning of netmask? There are total 32 bits in networking starting from all zeros to all ones. Netmask is a way of shorthand for referring to ranges of consecutive IP addresses in the Internet Protocol. The example we see here 192.168.29 falls under netmask of 255.255.255. The router can provide IP ranges from 192.168.29.0 to 192.168.29.255.

There will be administrative work so no one will get IP address of 192.168.29.0 and 192.168.29.255 so valid number of IP address space comes down to 253 numbers. Router (or DHCP provider) gave two addresses to two systems as 192.168.29.40 and 192.168.29.32 and they can communicate with each other in the network. You might need to visit other sites where does you need to send your packet?

DHCP server not only sends you IP address but a default gateway and DNS servers. Default gateway will be in the same subnet as 192.168.29.0 but your system sends any packet to gateway if IP is not matching 192.168.29.0 network. DNS servers will be explained later.

## Application protocols

We discussed DHCP protocol, which gives IP address, Default gateway, DNS servers to the system. We cover few of the application level protocols.

- DHCP
- HTTP
- HTTPS
- FTP
- DNS
- SSH
- SMTP
- POP3
- IMAP
- NTP

These are application protocols you will use or your system use regularly. Let us take example of NTP, stands for network time protocol which keeps your system time. Many application will not function if system time drifts time.

What is meant by protocol: You might hear in diplomatic circles about protocols. A protocol is mutually agreed way of communicating with each other. If one is agreed then where does it agreed? If you are writing the any of the above server application listed above, how does your server gets request and what information it will be sent?

RFC stands for Request for comment has information about what is allowed, what is not allowed. RFC 2616 and RFC 7231 cover about HTTP Protocol. You can google for NTP RFC and read about it.

Question: I wanted to run HTTP server how to run it? HTTP server has multipl applications one is apache httpd is most famous on Linux and Windows environment, Ngnix and lighttpd are other http servers. 
SSH server will come by default on Linux/Unix systems. 

We seen many application protocols, it is easy for humans to remember them but systems easier to have numbers. An application listed above running will be tied to port number. HTTP - 80, HTTPS - 443, SMTP - 25, SSH - 22, TELNET -22 etc. Linux have /etc/services file listed number and application, windows have c:\windows\system32\drivers\etc\services file for listing application to numbers matching.

## TCP, UDP and ICMP

ICMP stands for internet control message protocol, used for control message rather than regular message. All of you know famous ping command which is icmp message.

```
ping -c 10 www.google.com
PING www.google.com (142.250.192.68): 56 data bytes
64 bytes from 142.250.192.68: icmp_seq=0 ttl=54 time=43.445 ms
64 bytes from 142.250.192.68: icmp_seq=1 ttl=54 time=42.616 ms
64 bytes from 142.250.192.68: icmp_seq=2 ttl=54 time=43.989 ms
64 bytes from 142.250.192.68: icmp_seq=3 ttl=54 time=44.359 ms
64 bytes from 142.250.192.68: icmp_seq=4 ttl=54 time=42.599 ms
64 bytes from 142.250.192.68: icmp_seq=5 ttl=54 time=44.360 ms
64 bytes from 142.250.192.68: icmp_seq=6 ttl=54 time=43.658 ms
64 bytes from 142.250.192.68: icmp_seq=7 ttl=54 time=43.712 ms
64 bytes from 142.250.192.68: icmp_seq=8 ttl=54 time=42.755 ms
64 bytes from 142.250.192.68: icmp_seq=9 ttl=54 time=75.976 ms

--- www.google.com ping statistics ---
10 packets transmitted, 10 packets received, 0.0% packet loss
round-trip min/avg/max/stddev = 42.599/46.747/75.976/9.763 ms
```
We only pinged www.google.com, but we got IP address and more information.

- 10 packets sent and 10 got back no loss of packets. If loss of packets are there it is a problem in the network.
- RTT time is 46ms average.

Let us do one more site ping request. I am cancelling noise in the command with -q command.

```
 ping -q -c 10 www.purdue.edu
PING www.purdue.edu (128.210.7.200): 56 data bytes

--- www.purdue.edu ping statistics ---
10 packets transmitted, 10 packets received, 0.0% packet loss
round-trip min/avg/max/stddev = 273.510/317.480/385.833/40.180 ms
```
Notice that there is no packet loss but RTT here is 273ms not 40ms. 

Note: Some sites block icmp echo requests for security purpose. 
Exercize: Why is term ping came into picture? Read about ping wikipedia article.

### TCP

TCP stands for Transmission control protocol. TCP ensures packets are reliably delivered and sequenced over the internet. TCP has three way handshake and four way closing procedure.

![TCP 3 way handshake](./tcp1.png?raw=true "TCP3way")

![TCP 4 way close](./tcp2.png?raw=true "TCP4way")

Other features of TCP is it is reliable protocol to send data. If a packet didn't get ack, it will be sent again. We will cover about other aspects later point of time.

### UDP
The User Datagram Protocol (UDP) is a lightweight data transport protocol that works on top of IP.
UDP provides a mechanism to detect corrupt data in packets, but it does not attempt to solve other problems that arise with packets, such as lost or out of order packets. That's why UDP is sometimes known as the Unreliable Data Protocol.
UDP is simple but fast, at least in comparison to other protocols that work over IP. It's often used for time-sensitive applications (such as real-time video streaming) where speed is more important than accuracy.


## Recap of application protocol and Transport protocol

Q: If UDP is not reliable then why does one use it. 
Ans: TCP require 3 way handshake and 4 way closing for many applications it is an overhead. We don't need such heavyweight, they select UDP protocol.

We covered application protocols and transmission protocols (leave out icmp), how do they relate?

- HTTP HTTPS Uses TCP as tranmission protocol
- SSH, SMTP, POP3 ETC TCP
- DNS UDP (One component uses tcp but that is optional)
- NTP, DHCP - UDP

We can relate to this as 

HTTP uses TCP and runs on port 80. Can we run on port 8081 or 82 or 100? If no service is running on that port one can run on any free port with condition that remote users need to know the port (big thing) and it works like http://myside:82/ and your search engined might not be able to reach as they will not know non standard port.

You can run service on any port non standard but it is not a good idea as you need to advertise the port and service.

Excercise: 
i) Install a simple http server (download from apache httpd) and write a simple html message.
ii) Run a apache web server on a different port say 8080 and connect from your browser.




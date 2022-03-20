## Misc tools

### tcpdump
We discussed so far application level, transport level (tcp/udp) and routing (IP protocols). Is it possible to see the traffic and examine? Linux and windows has libpcap toolset to examine traffice. Windows you can install npcap and Linux tcpdump. Wireshark is a gui tool available to examine the tcpdump data. 

```
$tcpdump port 53
tcpdump: ioctl(SIOCIFCREATE): Operation not permitted
```
tcpdump requires root-level access permissions to get the data.

```
sudo tcpdump port 53 -vvv
tcpdump: data link type PKTAP
tcpdump: listening on pktap, link-type PKTAP (Apple DLT_PKTAP), capture size 262144 bytes
07:47:25.385264 IP (tos 0x0, ttl 64, id 40166, offset 0, flags [none], proto UDP (17), length 65)
    192.168.29.40.56822 > one.one.one.one.domain: [udp sum ok] 53497+ A? dnszombie.cisco.com. (37)
07:47:25.396803 IP (tos 0x0, ttl 53, id 59033, offset 0, flags [DF], proto UDP (17), length 116)
    one.one.one.one.domain > 192.168.29.40.56822: [udp sum ok] 53497 NXDomain q: A? dnszombie.cisco.com. 0/1/0 ns: cisco.com. [18m40s] SOA ns1.cisco.com. postmaster.cisco.com. 27484396 7200 1800 864000 1800 (88)
07:47:25.399552 IP (tos 0x0, ttl 64, id 42497, offset 0, flags [none], proto UDP (17), length 65)
    192.168.29.40.50805 > dns9.quad9.net.domain: [udp sum ok] 20382+ A? dnszombie.cisco.com. (37)
07:47:25.445261 IP (tos 0x0, ttl 50, id 12820, offset 0, flags [none], proto UDP (17), length 116)
    dns9.quad9.net.domain > 192.168.29.40.50805: [udp sum ok] 20382 NXDomain q: A? dnszombie.cisco.com. 0/1/0 ns: cisco.com. [17m32s] SOA ns1.cisco.com. postmaster.cisco.com. 27484396 7200 1800 864000 1800 (88)
07:47:25.448166 IP (tos 0x0, ttl 64, id 41729, offset 0, flags [none], proto UDP (17), length 74)
    192.168.29.40.54243 > one.one.one.one.domain: [udp sum ok] 48879+ [1au] TXT? debug.opendns.com. ar: . OPT UDPsize=1280 (46)
```
We will cover the reading of this data see the type of packet UDP and length protocol in this.
Note 1: An intermediate between you and the remote website (or your office administrator) can understand your browser behavior and read the data.
Note 2: HTTPS data is encrypted form (and many other application protocols), one can see where you are visiting but will not be able to read the data.

### SSH
SSH is an application protocol used in Linux/Solaris/Unix kind of operating systems only. SSH stands for secure shell host and way to connect to a remote host. 
One needs to have username and password to access remote host.
```
ssh root@172.26.231.84
Activate the web console with: systemctl enable --now cockpit.socket

This system is not registered to Red Hat Insights. See https://cloud.redhat.com/
To register this system, run: insights-client --register

Last login: Thu Feb  3 22:47:40 2022 from 172.26.230.30
[root@samredhat801 ~]# whoami
root
[root@samredhat801 ~]# ls
a.sh                                      nohup.out
anaconda-ks.cfg                           samredhat801.sh
flows                                     samredhat801.sh.orig
freerdp-2.0.0-46.rc4.el8_2.2.aarch64.rpm  security
hosts                                     somefile
initial-setup-ks.cfg                      sycg.pub
intent_service_linux                      tetration_installer_default_enforcer_windows_esx-3048.ps1
mongo.toml
[root@samredhat801 ~]#
```

Note1: We will not cover encryption here if you notice the remote host didn't prompt the password. How is it possible?

Linux hosts you can generate public and private keys and copy the public key to the remote host, if you use public/private keys you don't need to use password.
putty is the client used to connect remote hosts and puttygen is used for key generation. 


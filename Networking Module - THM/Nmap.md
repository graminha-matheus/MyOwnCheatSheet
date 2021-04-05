## Nmap

#### Introduction

When it comes to hacking, knowledge is power. The more knowledge you have about a target system or network, the more options you have available. This makes it 
imperative that proper enumeration is carried out before any exploitation attempts are made.

Say we have been given an IP (or multiple IP addresses) to perform a 
security audit on. Before we do anything else, we need to get an idea of
 the “landscape” we are attacking. What this means is that we need to 
establish which services are running on the targets. For example, 
perhaps one of them is running a webserver, and another is acting as a 
Windows Active Directory Domain Controller. 

The first stage in establishing this “map” of the landscape is something called port 
scanning. When a computer runs a network service, it opens a networking 
construct called a “port” to receive the connection.  

Ports are necessary for making multiple network requests or having multiple 
services available. For example, when you load several webpages at once in a web browser, the program must have some way of determining which tab is loading which web page. This is done by establishing connections to the remote webservers using different ports on your local machine. Equally, if you want a server to be able to run more than one service (for example, perhaps you want your webserver to run both HTTP and HTTPS versions of the site), then you need some way to direct the traffic to the appropriate service. Once again, ports are the solution to this. 

Network connections are made between two ports – an open port listening 
on the server and a randomly selected port on your own computer. For example, when you connect to a web page, your computer may open port 49534 to connect to the server’s port 443.

<img src="https://i.imgur.com/3XAfRpI.png" title="" alt="" data-align="center">

As in the previous example, the diagram shows what happens when you connect to numerous websites at the same time. Your computer opens up a different, high-numbered port (at random), which it uses for all its communications with the remote server.

Every computer has a total of 65535 available ports; however, many of these are registered as standard ports. 

For example:

- **A HTTP Webservice can nearly always be found on port 80 of the server**.

- **A HTTPS Webservice can be found on port 443**.

- **Windows NETBIOS can be found on port 139 and SMB can be found on port 445**. 

It is important to note; however, that especially in a CTF setting, **it is not unheard of for even these standard ports to be altered**, making it even more imperative that we 
perform appropriate enumeration on the target.

If we do not know which of these ports a server has open, then we do not have a hope of successfully attacking the target; thus, it is crucial that we begin any attack with a port scan. This can be accomplished in a variety of ways – usually using a tool called nmap, which is the focus of this room. 

Nmap can be used to perform many different kinds of port scan – the most common of these will be introduced in upcoming tasks; however, the basic theory is this: nmap 
will connect to each port of the target in turn. 

Depending on how the port responds, it can be determined as being open, closed, or filtered (usually by a firewall). Once we know which ports are open, we can then 
look at enumerating which services are running on each port – either 
manually, or more commonly using nmap.

So, why nmap? The short answer is that it's currently the industry standard for a reason: no other port scanning tool comes close to matching its functionality (although some newcomers are now matching it for speed). It is an extremely powerful tool – made even more powerful by its scripting engine which can be used to scan for vulnerabilities, and in some cases even perform the exploit directly! Once again, this 
will be covered more in upcoming tasks.

For now, it is important that you understand: what port scanning is; why it is necessary; and that nmap is the tool of choice for any kind of initial enumeration.

---

#### Scan Types Overview

When port scanning with Nmap, there are three basic scan types. These are:

- TCP Connect Scans (`-sT`)
- SYN "Half-open" Scans (`-sS`)
- UDP Scans (`-sU`)

Additionally there are several less common port scan types, some of which we will also cover (albeit in less detail). These are:

- TCP Null Scans (`-sN`)
- TCP FIN Scans (`-sF`)
- TCP Xmas Scans (`-sX`)

Most of these (with the exception of UDP scans) are used for very 
similar purposes, however, the way that they work differs between each 
scan. This means that, whilst one of the first three scans are likely to
 be your go-to in most situations, it's worth bearing in mind that other
 scan types exist.

In terms of network scanning, we will also look briefly at ICMP (or "ping") scanning.

---

#### TCP Connect Scans

To understand TCP Connect scans (`-sT`), it's important that you're comfortable with the *TCP three-way handshake*. If this term is new to you then completing [Introductory Networking](https://tryhackme.com/room/introtonetworking) before continuing would be advisable.

As a brief recap, the three-way handshake consists of three stages. 
First the connecting terminal (our attacking machine, in this instance) 
sends a TCP request to the target server with the SYN flag set. The 
server then acknowledges this packet with a TCP response containing the 
SYN flag, as well as the ACK flag. Finally, our terminal completes the 
handshake by sending a TCP request with the ACK flag set.  
<img src="https://muirlandoracle.co.uk/wp-content/uploads/2020/03/image-2.png" title="" alt="" data-align="center"><img src="https://i.imgur.com/ngzBWID.png" title="" alt="" data-align="center">

This is one of the fundamental principles of TCP/IP networking, but how does it relate to Nmap?

Well, as the name suggests, a TCP Connect scan works by performing 
the three-way handshake with each target port in turn. In other words, 
Nmap tries to connect to each specified TCP port, and determines whether
 the service is open by the response it receives.

---

For example, if a port is closed, [RFC 793](https://tools.ietf.org/html/rfc793) states that:

*"... If the connection does not exist (CLOSED) then a reset is 
sent in response to any incoming segment except another reset.  In 
particular, SYNs addressed to a non-existent connection are rejected by 
this means."*

In other words, if Nmap sends a TCP request with the *SYN* flag set to a ***closed*** port, the target server will respond with a TCP packet with the *RST* (Reset) flag set. By this response, Nmap can establish that the port is closed.

<img src="https://i.imgur.com/vUQL9SK.png" title="" alt="" data-align="center">

If, however, the request is sent to an *open* port, the target will respond with a TCP packet with the SYN/ACK flags set. Nmap then marks this port as being *open* (and completes the handshake by sending back a TCP packet with ACK set).

---

This is all well and good, however, there is a third possibility.

What if the port is open, but hidden behind a firewall?

Many firewalls are configured to simply **drop** 
incoming packets. Nmap sends a TCP SYN request, and receives nothing 
back. This indicates that the port is being protected by a firewall and 
thus the port is considered to be *filtered*.

That said, it is very easy to configure a firewall to respond with a 
RST TCP packet. For example, in IPtables for Linux, a simple version of 
the command would be as follows:

`iptables -I INPUT -p tcp --dport <port> -j REJECT --reject-with tcp-reset`

This can make it extremely difficult (if not impossible) to get an accurate reading of the target(s).

---

#### SYN Connect Scans

As with TCP scans, SYN scans (`-sS`) are used to scan the 
TCP port-range of a target or targets; however, the two scan types work 
slightly differently. SYN scans are sometimes referred to as "*Half-open"* scans, or *"Stealth"* scans.  

Where
 TCP scans perform a full three-way handshake with the target, SYN scans
 sends back a RST TCP packet after receiving a SYN/ACK from the server 
(this prevents the server from repeatedly trying to make the request). 
In other words, the sequence for scanning an **open** port looks like this:

<img src="https://i.imgur.com/cPzF0kU.png" title="" alt="" data-align="center">

<img src="https://i.imgur.com/bcgeZmI.png" title="" alt="" data-align="center">

This has a variety of advantages for us as hackers:

- It
   can be used to bypass older Intrusion Detection systems as they are 
  looking out for a full three way handshake. This is often no longer the 
  case with modern IDS solutions; it is for this reason that SYN scans are
   still frequently referred to as "stealth" scans.
- SYN scans are
   often not logged by applications listening on open ports, as standard 
  practice is to log a connection once it's been fully established. Again,
   this plays into the idea of SYN scans being stealthy.
- Without 
  having to bother about completing (and disconnecting from) a three-way 
  handshake for every port, SYN scans are significantly faster than a 
  standard TCP Connect scan.

There are, however, a couple of disadvantages to SYN scans, namely:

- They require sudo permissions[1] in order to work correctly in Linux. This is because SYN scans require 
  the ability to create raw packets (as opposed to the full TCP 
  handshake), which is a privilege only the root user has by default.
- Unstable
   services are sometimes brought down by SYN scans, which could prove 
  problematic if a client has provided a production environment for the 
  test.

All in all, the pros outweigh the cons.

For this reason, SYN scans are the default scans used by Nmap *if run with sudo permissions*. If run **without** sudo permissions, Nmap defaults to the TCP Connect scan we saw in the previous task.

---

When using a SYN scan to identify closed and filtered ports, the exact same rules as with a TCP Connect scan apply.

If
 a port is closed then the server responds with a RST TCP packet. If the
 port is filtered by a firewall then the TCP SYN packet is either 
dropped, or spoofed with a TCP reset.

In this regard, the two scans are identical: the big difference is in how they handle *open* ports.

---

[1]
 SYN scans can also be made to work by giving Nmap the CAP_NET_RAW, 
CAP_NET_ADMIN and CAP_NET_BIND_SERVICE capabilities; however, this may 
not allow many of the NSE scripts to run properly.

---

#### UDP Scans

Unlike TCP, UDP connections are *stateless*. This means that, 
rather than initiating a connection with a back-and-forth "handshake", 
UDP connections rely on sending packets to a target port and essentially
 hoping that they make it. This makes UDP superb for connections which 
rely on speed over quality (e.g. video sharing), but the lack of 
acknowledgement makes UDP significantly more difficult (and much slower)
 to scan. The switch for an Nmap UDP scan is (`-sU`)  

When a packet is sent to an open UDP port, there should be no response. When this happens, Nmap refers to the port as being `open|filtered`.
 In other words, it suspects that the port is open, but it could be 
firewalled. If it gets a UDP response (which is very unusual), then the 
port is marked as *open*. More commonly there is no response, in 
which case the request is sent a second time as a double-check. If there
 is still no response then the port is marked *open|filtered* and Nmap moves on.  

When a packet is sent to a *closed* UDP port, the target should respond with an ICMP (ping) packet 
containing a message that the port is unreachable. This clearly 
identifies closed ports, which Nmap marks as such and moves on.  

---

Due
 to this difficulty in identifying whether a UDP port is actually open, 
UDP scans tend to be incredibly slow in comparison to the various TCP 
scans (in the region of 20 minutes to scan the first 1000 ports, with a 
good connection). For this reason it's usually good practice to run an 
Nmap scan with `--top-ports <number>` enabled. For example, scanning with  `nmap -sU --top-ports 20 <target>`. Will scan the top 20 most commonly used UDP ports, resulting in a much more acceptable scan time.

---

When
 scanning UDP ports, Nmap usually sends completely empty requests -- 
just raw UDP packets. That said, for ports which are usually occupied by
 well-known services, it will instead send a protocol-specific payload 
which is more likely to elicit a response from which a more accurate 
result can be drawn.

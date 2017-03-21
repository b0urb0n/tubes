# How the Internet Works - A Brain Dump
## (or "What happens when I go to www.google.com")

## Author: **bourbon**

### Introduction

TODO: Put more things here

I will focus on the Linux platform; specifically CentOS 6.7 x64.

***

### Prerequisite Information

#### OSI Model
By far, the most boring part of this whole paper but it must be done.

Layers:

TODO: figure out how to reverse this list (markdown doesn't like reverse numeric lists)

1. Physical
2. Data Link
3. Network
4. Transport
5. Session
6. Presentation
7. Application

As we move down the OSI model (from 7 to 1) our data gets encapsulated.  This is an important concept to understand because it happens all the time and it happens multiple times per network "conversation".

The best analogy for this would be wrapping a preseant for a friend.  Except, you decided to be an ass and wrap it multiple times.
*  You start with the present (the original data) and wrap (encapsulate) it.
*  Now it's a segment (TCP) or a datagram (UDP) depending on your transport protocol.  You wrap it again.
*  Now it's a packet.  You wrap it again.
*  Now it's a frame.

```
/-----Frame-------\
| /---Packet----\ |
| | /-Segment-\ | |
| | |  Data   | | |
| | \---------/ | |
| \-------------/ |
\-----------------/
```

##### Brief summary of each:
1. **Physical** - Cable/Medium - Literally 1's and 0's travelling over a medium (ie. cat-5, fiber, etc.)
    Commonly found types of cable:
    *  Cat(egory)-5 Ethernet - four variably twisted pairs of copper (variably twisting prevents signal attenuation between the pairs)
    *  Single-mode fiber - a pair of glass filaments that carries one beam of light in one direction (TX and RX)
    
2. **Data Link** - MAC addresses - Ethernet frame transmitted from one physical station to another
    An ethernet **frame** consists of a header, a payload (data), and a checksum.  The first 14 bytes are the header and contain the source and destination MAC address and the ether type.  The data portion is variable in length but normally 46-1500 bytes (depending on MTU).  The frame check sum (FCS) is 4 bytes long and is the result of digesting the entire frame with the CRC algorithm.  The FCS enables the receiving station to detect corruption in the frame.

3. **Network** - IP addresses - Ultimate source and ultimate destination stations (whereas Data Link is responsible for intermediate transmission)
    An IP(v4) **packet** is made up of a header and a payload (data).  The header has a lot of information including IP version (4/6), packet length, flags, protocol (of the payload; TCP/UDP/etc.), source and destination IP addresses, and optional options.

4. **Transport** - Ports - Protocols (ie. TCP/UDP/etc.) and their options
    The transport section normally includes things like port numbers, data, and error detection/correction (sometimes)

***

5. **Session** - Manages multiple exchanges of information (often at the same time)
    
6. **Presentation** - Compression, Encryption, Encoding, etc.

7. **Application** - Interface between the software and system/API calls used to access the network

***

### Core Information

#### Generic new connection flow

***

#### TCP

***

#### UDP

***

#### www.google.com example

1. Enter URL in browser
2. Firefox disects URL
    * Three parts (protocol, host, document)
    * https://www.google.com/maps
        * protocol: HTTPS
        * host: www.google.com
            * host: www
            * domain: google
            * top level domain (TLD): com
        * document: /maps (/ is the root directory from the perspective of the webserver process)
3. DNS lookup www.google.com
    * Decide on a method of lookup (in order)
    * `/etc/hosts` - is there a static definition of www.google.com (usually not)?
    * `/etc/resolv.conf` - this is where you define your DNS servers (usually your router or 8.8.8.8)
        * Request to the nameservers (in order) for com.google.www (backwards on purpose)
            * First, we talk to `com` to figure out who is responsible for the `google` domain
            * Second, we talk to the `google` domain to get an address for the `www` host (in this case the front end of a load-balancer, but that's another topic)
    * An IP address associated with `www.google.com` is the end result of this section (172.217.1.36)
4. HTTP request to 172.217.1.36 on port 80/443
    * Establish TCP connection to 172.217.1.36 on port 443 (because HTTPS)
    * HTTP GET request for the document above (/maps)
    * A blob of HTML (among other things (JavaScript, CSS, etc.)) is the result of this section
5. Render HTML


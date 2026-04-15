# Networking & The Internet — How Computers Talk to Each Other

> A single computer can compute. A billion computers, connected, can change civilization.
> Understanding networking means understanding the invisible infrastructure that modern life
> depends on — every search query, every video call, every bank transaction flows through
> the ideas in this chapter.

**Prerequisites:** [Operating Systems](../02-systems/operating-systems.md) — You need processes, files, and system calls.

**What it enables:** The web, distributed systems, cloud computing, everything connected.

---

## 1. Why Networking Exists

A computer by itself is useful. A computer connected to every other computer on Earth is
*transformative*. That's the fundamental insight behind networking.

The internet is the most complex machine humanity has ever built. It connects roughly 5
billion people, tens of billions of devices, and carries exabytes of data per day. It has
no central control, no single owner, and no master switch. It evolved from a handful of
university computers in the 1960s (ARPANET) to the backbone of modern civilization.

The core challenge: **how do you get a chunk of data from point A to point B, reliably,
efficiently, and securely, across a planet-spanning web of heterogeneous hardware?**

The answer, as with most hard engineering problems, is *layers of abstraction*.

---

## 2. The Layered Model

### Why Layers?

You could, in theory, design one monolithic protocol that handles everything from
electrical signaling to web page rendering. It would be unmaintainable, unextensible, and
impossible to reason about. Instead, networking is organized into **layers**, each solving
exactly one problem and providing a clean interface to the layer above.

The two layered models you'll encounter are:

- **TCP/IP model** (4-5 layers) — the practical model the internet actually uses
- **OSI model** (7 layers) — a theoretical model that's useful for teaching but nobody
  implements exactly

We'll use the TCP/IP model because it maps to reality. Here are the layers, bottom to top:

### Physical Layer: Bits on a Wire

The lowest layer deals with raw bits — ones and zeros — traveling through a physical
medium. This is where physics meets computer science.

- **Ethernet cables** (Cat5, Cat6): electrical signals over copper
- **Fiber optics**: pulses of light through glass fibers (fast, long-distance)
- **WiFi (802.11)**: radio waves through air
- **Cellular (4G/5G)**: radio waves over longer distances

The physical layer doesn't know what the bits mean. Its job is: "get these bits to the
other end of this physical connection."

### Link Layer: Frames Between Adjacent Nodes

The link layer connects devices that are *physically adjacent* — on the same local network.
It organizes raw bits into **frames** and handles addressing within a local network.

Key concepts:
- **MAC addresses**: 48-bit hardware addresses burned into every network card
  (e.g., `a4:83:e7:2d:6b:01`). Globally unique identifiers for devices.
- **ARP (Address Resolution Protocol)**: translates IP addresses to MAC addresses.
  "I know the IP, but which physical device on this network has it?"
- **Switches**: forward frames based on MAC addresses. Unlike dumb hubs (which broadcast
  everything everywhere), switches learn which device is on which port.

### Network Layer: Packets Across Networks

This is where the *inter*-net happens. The network layer gets data across *different*
networks — from your home WiFi, through your ISP, across backbone networks, to a server
in another continent. The star is **IP (Internet Protocol)**, covered in depth next.

Key concepts: **IP addresses** (logical addresses across the global internet), **routers**
(forward packets between networks), and **routing** (algorithms determining which path a
packet takes).

### Transport Layer: Reliable Streams or Fast Datagrams

The network layer delivers packets, but it makes no promises. Packets can arrive out of
order, get duplicated, or vanish entirely. The transport layer sits on top and provides
either:

- **TCP**: reliable, ordered streams (the workhorse of the internet)
- **UDP**: fast, unreliable datagrams (for when speed matters more than reliability)

This layer also introduces **ports** — numbers (0-65535) that identify which *application*
on a machine should receive the data. IP gets data to the right machine; ports get it to
the right application. Your web browser might use port 443 (HTTPS), while your email
client uses port 587 (SMTP).

### Application Layer: Protocols Apps Use

The top layer is where application-specific protocols live:

- **HTTP/HTTPS**: the web
- **DNS**: translating domain names to IP addresses
- **SMTP/IMAP**: email
- **SSH**: secure remote shell access
- **FTP**: file transfer (mostly legacy now)

These protocols define the *meaning* of the data being exchanged.

### Encapsulation: The Russian Nesting Doll

As data travels down the layers, each layer wraps the data from the layer above with its
own header (and sometimes trailer). This is **encapsulation**:

```
Application:  [        DATA        ]
Transport:    [ TCP header |  DATA  ]           ← segment
Network:      [ IP header | TCP header | DATA ] ← packet
Link:         [ ETH header | IP | TCP | DATA | ETH trailer ] ← frame
Physical:     101001110100011101001...           ← bits
```

When data arrives at the destination, the process reverses: each layer strips its header
and passes the payload up to the next layer. This is why layers work — each layer only
needs to understand its own header.

---

## 3. IP Addresses and Routing

### IPv4

Every device on the internet needs an address. IPv4 uses **32-bit addresses**, written as
four decimal octets: `192.168.1.42`.

32 bits means approximately **4.3 billion** possible addresses. That seemed like plenty in
the 1980s. It is very much not plenty now. We've been working around this limitation for
decades.

### IPv6

The long-term fix: **128-bit addresses**, written in hexadecimal:
`2001:0db8:85a3:0000:0000:8a2e:0370:7334`

128 bits gives us approximately 3.4 x 10^38 addresses — enough for every atom on Earth's
surface to have its own IP. IPv6 adoption is gradual but accelerating. Most of the
internet still runs on IPv4, with IPv6 running alongside it.

### Subnets and CIDR Notation

Not every device needs to know about every other device. IP addresses are divided into
**network** and **host** portions using subnet masks.

**CIDR (Classless Inter-Domain Routing)** notation expresses this compactly:
- `192.168.1.0/24` means the first 24 bits are the network, the last 8 identify hosts.
  That's 256 addresses (192.168.1.0 through 192.168.1.255).
- `10.0.0.0/8` means the first 8 bits are the network — that's 16.7 million addresses.

### NAT: Sharing One Public IP

Here's a fact that surprises people: most devices don't have a public IP address.
Your home router has one public IP. Every device behind it — your phone, laptop, smart
speaker, that questionable IoT toaster — shares that one address using **NAT (Network
Address Translation)**.

NAT works by rewriting packet headers. When your laptop sends a request, the router
replaces the private source IP (`192.168.1.42`) with the public IP (`73.45.201.8`) and
records the mapping. When the response comes back, it reverses the translation. It's a
hack, but a remarkably effective one — and one reason IPv6 adoption has been slow.

### How Routing Works

When you send a packet to `142.250.80.46` (a Google server), your laptop doesn't know the
full path. It knows one thing: "send it to my default gateway" (your home router). Your
router forwards it to your ISP. Your ISP knows more. Hop by hop, the packet reaches its
destination.

Each router maintains a **routing table** — rules mapping destination IP ranges to "next
hops." The router looks up the destination, finds the best match, and forwards the packet.

**BGP (Border Gateway Protocol)** is the protocol that glues the internet together. It's
how **autonomous systems** (large networks, typically ISPs or big companies) exchange
routing information. BGP is remarkably simple for the job it does, and remarkably
fragile — a misconfiguration by one ISP can (and occasionally does) take down large
portions of the internet.

---

## 4. TCP — Reliable Streams

TCP (Transmission Control Protocol) is the backbone of reliable internet communication.
Web browsing, email, file transfers, API calls — they all use TCP.

### The Three-Way Handshake

Before any data flows, TCP establishes a connection:

```
Client                    Server
  |                         |
  |--- SYN (seq=100) ----->|     "I want to connect, starting at sequence 100"
  |                         |
  |<-- SYN-ACK (seq=300, --|     "OK, I acknowledge your 100, I'm starting at 300"
  |    ack=101)             |
  |                         |
  |--- ACK (ack=301) ----->|     "Got it, acknowledged your 300"
  |                         |
  |   Connection established |
```

This ensures both sides are ready and agree on initial sequence numbers. It adds one
round-trip of latency before any data flows — a cost TCP pays for reliability.

### Sequence Numbers and Acknowledgements

Every byte TCP sends has a **sequence number**. The receiver sends back
**acknowledgements**: "I've received everything up to byte X, send me X+1 next."

If the sender doesn't get an acknowledgement within a timeout, it **retransmits**. The
receiver uses sequence numbers to reorder bytes and discard duplicates.

### Flow Control: The Sliding Window

What if the sender is faster than the receiver? The receiver would be overwhelmed. TCP
handles this with **flow control** using a sliding window.

The receiver advertises a **window size**: "I have room for N more bytes." The sender
never sends more than the window allows. As the receiver processes data, it slides the
window forward, allowing more data to be sent.

This is a collaborative mechanism — the receiver controls the pace.

### Congestion Control: Don't Overwhelm the Network

Flow control prevents overwhelming the *receiver*. But what about the *network itself*?
If everyone blasts data at full speed, routers overflow and drop packets, making things
worse for everyone.

TCP's congestion control algorithms prevent this:

- **Slow start**: Begin sending slowly. Double the sending rate each round trip until
  you detect congestion (packet loss).
- **AIMD (Additive Increase, Multiplicative Decrease)**: Once at cruising speed, increase
  the sending rate gradually. When congestion is detected, cut the rate in half.

No central authority tells anyone how fast to send. Each TCP connection independently
probes the network and backs off when it detects trouble.

### The Guarantees and Their Cost

TCP guarantees:
- **Delivery**: every byte you send arrives (or you get an error)
- **Order**: bytes arrive in the order they were sent
- **No duplication**: you won't get the same data twice

The cost:
- **Latency**: the handshake adds one round trip before data flows
- **Head-of-line blocking**: if one packet is lost, everything behind it waits
- **Overhead**: headers, acknowledgements, and retransmissions consume bandwidth

For most applications, the guarantees are worth the cost. But not always.

---

## 5. UDP — Fast Datagrams

UDP (User Datagram Protocol) is TCP's minimalist cousin. No handshake. No sequence
numbers. No acknowledgements. No retransmission. No guarantee of delivery, order, or
non-duplication.

UDP just takes your data, slaps a minimal header on it (source port, destination port,
length, checksum), and fires it into the network. What happens after that is not UDP's
problem.

### Why Would Anyone Want This?

Speed. For some applications, TCP's guarantees are not just unnecessary — they're actively
harmful.

**Video/audio streaming**: If a video frame arrives late because TCP is retransmitting a
lost packet, you see a stutter. Better to skip the lost frame and keep playing. The viewer
won't notice a single missing frame, but they'll definitely notice a freeze.

**Online gaming**: If your character's position update is lost, you don't want to wait for
retransmission. The next update will arrive in 16 milliseconds anyway with the current
position.

**DNS queries**: A simple request-response pair. If it's lost, just send it again. No need
for the overhead of a TCP connection.

**VoIP (Voice over IP)**: Same reasoning as video. Late audio is useless audio.

### UDP in Practice

Many applications that use UDP build their own reliability mechanisms *on top* of it —
only the specific guarantees they need. QUIC (which powers HTTP/3) does exactly this:
it runs over UDP but implements its own reliability, congestion control, and multiplexing.

UDP gives you a raw building block. You decide what guarantees to add.

---

## 6. DNS — The Phone Book of the Internet

Nobody wants to type `142.250.80.46` to visit Google. Humans think in names; computers
think in numbers. **DNS (Domain Name System)** bridges the gap.

### The Hierarchy

DNS is distributed and hierarchical. When you type `www.google.com`, here's what happens:

```
Your computer → "What's the IP for www.google.com?"
      ↓
Local DNS resolver (your ISP or 8.8.8.8)
      ↓ (if not cached)
Root server → "I don't know, but .com is handled by these TLD servers"
      ↓
.com TLD server → "I don't know, but google.com is handled by these nameservers"
      ↓
Google's authoritative nameserver → "www.google.com is 142.250.80.46"
```

There are **13 root server addresses** (actually hundreds of physical servers using
anycast behind them).

### DNS Record Types

| Record | Purpose | Example |
|--------|---------|---------|
| **A** | Name → IPv4 address | `google.com → 142.250.80.46` |
| **AAAA** | Name → IPv6 address | `google.com → 2607:f8b0:4004:...` |
| **CNAME** | Alias to another name | `www.example.com → example.com` |
| **MX** | Mail server for domain | `gmail.com → alt1.gmail-smtp-in.l.google.com` |
| **NS** | Nameserver for domain | `google.com → ns1.google.com` |
| **TXT** | Arbitrary text (used for verification, SPF, etc.) | Various |

### Caching and TTL

DNS lookups would be painfully slow if every request went to the root. Instead, results
are **cached** at multiple levels — your browser, your OS, your ISP's resolver.

Each DNS record has a **TTL (Time to Live)** — how many seconds the record can be cached
before it must be re-queried. A TTL of 3600 means "cache this for one hour." Short TTLs
enable quick changes; long TTLs reduce load.

### Critical and Fragile

DNS is one of the internet's single points of vulnerability. If DNS goes down, the
internet effectively goes down — not because networks stop working, but because nobody can
find anything. Major DNS outages (like the 2021 Fastly incident or the 2016 Dyn DDoS
attack) have taken huge swaths of the internet offline.

---

## 7. HTTP/HTTPS — The Language of the Web

HTTP (Hypertext Transfer Protocol) is the protocol your browser speaks. It's a
**request/response** protocol: the client sends a request, the server sends back a
response.

### The Anatomy of a Request

```
GET /search?q=networking HTTP/1.1
Host: www.google.com
User-Agent: Mozilla/5.0
Accept: text/html
```

- **Method**: what you want to do (GET, POST, PUT, DELETE, PATCH, etc.)
- **Path**: what resource you want (`/search?q=networking`)
- **Headers**: metadata about the request

### The Anatomy of a Response

```
HTTP/1.1 200 OK
Content-Type: text/html; charset=UTF-8
Content-Length: 48234

<!DOCTYPE html>
<html>...
```

### Status Codes

The server tells you what happened with a three-digit code:

| Range | Meaning | Common Codes |
|-------|---------|-------------|
| **2xx** | Success | 200 OK, 201 Created, 204 No Content |
| **3xx** | Redirect | 301 Moved Permanently, 304 Not Modified |
| **4xx** | Client error | 400 Bad Request, 401 Unauthorized, 403 Forbidden, 404 Not Found |
| **5xx** | Server error | 500 Internal Server Error, 502 Bad Gateway, 503 Service Unavailable |

### The Evolution of HTTP

**HTTP/1.1** (1997): Text-based. One request at a time per connection. To load a web page
with 50 resources, you needed many connections or waited a long time.

**HTTP/2** (2015): Binary framing. **Multiplexing** — multiple requests and responses
interleaved over a single connection. Header compression. Server push. Dramatically
faster for complex web pages.

**HTTP/3** (2022): Runs over **QUIC** (which runs over UDP instead of TCP). Eliminates
TCP's head-of-line blocking. Faster connection establishment (0-RTT in some cases). Built
for the modern, mobile, high-latency internet.

### REST APIs

REST (Representational State Transfer) is a pattern for designing web APIs using HTTP:
- Resources identified by URLs (`/users/42`, `/posts/17/comments`)
- HTTP methods map to operations (GET = read, POST = create, PUT = update, DELETE = remove)
- Stateless: each request contains everything the server needs

REST isn't a protocol — it's a set of conventions that make HTTP-based APIs predictable
and consistent.

---

## 8. TLS/SSL — Encryption in Transit

Plain HTTP sends everything in cleartext. Anyone between you and the server — your ISP,
the coffee shop WiFi operator, a government — can read every byte. **TLS (Transport Layer
Security)** fixes this.

### The TLS Handshake

The problem: how do two parties who've never met agree on an encryption key without an
eavesdropper learning it?

The solution uses **asymmetric cryptography** (public/private key pairs):

```
Client                              Server
  |                                    |
  |--- ClientHello ------------------>|  "I support these cipher suites"
  |                                    |
  |<-- ServerHello, Certificate ------|  "Let's use this cipher, here's my cert"
  |                                    |
  |  [Client verifies certificate     |
  |   against trusted CAs]            |
  |                                    |
  |--- Key Exchange ----------------->|  "Here's material to derive our shared key"
  |                                    |
  |  [Both derive the same            |
  |   symmetric session key]          |
  |                                    |
  |<== Encrypted communication ======>|  All further data is encrypted
```

After the handshake, both sides have the same symmetric key (like AES), which is fast
for bulk encryption. The asymmetric crypto was only used to establish that key securely.

### Certificate Authorities and PKI

How does the client know the server is who it claims to be? **Certificate authorities
(CAs)** — trusted third parties that vouch for a server's identity.

The server presents a **certificate** signed by a CA. Your browser has a built-in list of
trusted CAs. If the certificate is signed by a trusted CA and matches the domain, the
browser trusts the connection. This is **Public Key Infrastructure (PKI)**.

### Why HTTPS Matters

HTTPS (HTTP over TLS) provides three things:
- **Encryption**: eavesdroppers can't read the data
- **Authentication**: you're talking to the real server, not an impostor
- **Integrity**: data can't be tampered with in transit

This is why the industry has pushed for HTTPS everywhere. It's no longer just for
banks — it's for every website.

---

## 9. Sockets — The Programming Interface

All of this networking eventually hits an API that programmers use. That API is
**sockets** — the fundamental interface for network communication in virtually every
programming language.

A socket is an endpoint for communication — a file descriptor (on Unix, literally) where
the other end is a remote machine.

### The Server Flow

```
socket()    → Create a socket
bind()      → Attach it to an address and port
listen()    → Start accepting connections
accept()    → Wait for a client (returns a new socket for that client)
recv/send() → Exchange data
close()     → Clean up
```

### The Client Flow

```
socket()    → Create a socket
connect()   → Connect to the server's address and port
send/recv() → Exchange data
close()     → Clean up
```

### Putting It Together

```
Server                          Client
  |                               |
  | socket()                      |
  | bind(port 8080)               |
  | listen()                      |
  |                               | socket()
  |                               | connect(server:8080)
  | accept() ← connection -----→ |
  |        new client socket      |
  |                               | send("GET / HTTP/1.1...")
  | recv() ← "GET / HTTP/1.1..." |
  | send("HTTP/1.1 200 OK...")    |
  |              → recv() -----→  | recv() ← "HTTP/1.1 200 OK..."
  | close()                       | close()
```

Every web server, database connection, and multiplayer game uses this pattern at its core.
Libraries add abstraction, but underneath it's always `socket()`, `connect()`, `send()`,
`recv()`.

---

## 10. The Real Infrastructure

The layered model is clean. The actual internet is a sprawling physical system of cables,
buildings, and hardware.

### CDNs (Content Delivery Networks)

When someone in Tokyo requests an image from a Virginia server, the speed of light alone
adds ~70ms each way. **CDNs** (Cloudflare, Amazon CloudFront, Akamai) solve this by
caching content in servers distributed worldwide. That image gets cached at a Tokyo edge
server — milliseconds instead of hundreds.

### Load Balancers

No single server can handle millions of requests. **Load balancers** distribute incoming
traffic across a fleet of servers. They can work at different layers:

- **L4 (transport)**: routes based on IP and port, very fast
- **L7 (application)**: inspects HTTP headers, can make smarter routing decisions

They also handle health checks — if a server goes down, the load balancer stops sending
it traffic.

### BGP and Peering

The internet is not one network — it's a network of networks. ISPs and large organizations
run **autonomous systems (ASes)**, each with their own internal routing.

These ASes connect through **peering** arrangements — either at public internet exchange
points (IXPs) or through direct private connections. BGP is the protocol they use to
announce which IP ranges they can reach.

When you trace the path from your laptop to a server in another country, the packet
might pass through 5-10 different autonomous systems, each making independent routing
decisions.

### Submarine Cables

This surprises people: the vast majority of intercontinental internet traffic travels
through **undersea fiber optic cables** laid on the ocean floor. Not satellites. Cables.

Over 500 submarine cables span more than 1.4 million kilometers, carrying ~99% of
intercontinental data. Increasingly, they're owned by tech giants like Google, Meta, and
Microsoft. A single modern cable can carry tens of terabits per second.

### Data Centers

"The cloud" is just someone else's computer — organized into massive **data centers**
with thousands of servers. Major cloud providers (AWS, Google Cloud, Azure) operate them
globally, strategically located near cheap power, fiber routes, and cool climates.

---

## Key Mental Models

**The hourglass model**: IP is the narrow waist. Below it, any physical medium can carry
IP packets. Above it, any application protocol can run over IP. This is why the internet
is so adaptable — you can invent new physical technologies (WiFi, 5G) or new applications
(HTTP/3, WebRTC) without changing the core.

**End-to-end principle**: Keep the network core simple and dumb. Push complexity to the
endpoints. Routers should forward packets, not inspect or modify them (mostly). This
principle has guided internet design from the beginning and is a big reason it scaled.

**Best effort delivery**: IP makes no guarantees. It tries its best to deliver your
packet, but it might fail. Reliability is built on top, by the endpoints. This simplifies
the network enormously.

**Encapsulation**: Each layer wraps and unwraps. This separation of concerns is why a
WiFi engineer and a web developer can both do their jobs without understanding each other's
work.

**Names vs. addresses vs. routes**: DNS names are for humans, IP addresses are for
the network layer, MAC addresses are for the link layer, and routes are for routers. Each
serves a different purpose at a different level of abstraction.

---

## How This Connects

- **Operating systems** provide the socket API and manage network interfaces. The kernel
  implements TCP/IP. Everything in this chapter runs on the OS foundation from the previous
  chapter.
- **Distributed systems** (next chapter) build on networking to coordinate multiple
  machines. Consensus algorithms, replication, and fault tolerance all assume a network
  that can delay, drop, or reorder messages.
- **Security** intersects networking at every layer — from TLS to firewalls to DDoS
  protection.
- **Databases** use networking for replication, sharding, and client-server communication.
- **Web development** is built entirely on HTTP, DNS, and TLS. Understanding the
  underlying protocols makes you a dramatically better web developer.

---

## Hands-On

You can explore networking right now with tools that are probably already on your machine.

### curl — Make HTTP Requests

```bash
# Simple GET request
curl https://httpbin.org/get

# See the full request and response headers
curl -v https://httpbin.org/get

# POST with JSON body
curl -X POST https://httpbin.org/post -H "Content-Type: application/json" -d '{"key": "value"}'
```

### dig — Query DNS

```bash
# Look up the A record for google.com
dig google.com

# See the full resolution chain
dig +trace google.com

# Query a specific record type
dig MX gmail.com
dig AAAA google.com
```

### traceroute — See the Path Packets Take

```bash
# Trace the route to google.com
traceroute google.com

# Each line is a router hop, showing IP and round-trip times
```

### Wireshark — Inspect Packets

Wireshark captures and displays every packet flowing through your network interface.
Start a capture, open a web page, and watch the DNS query, TCP handshake, TLS handshake,
and HTTP exchange unfold in real time. Click any packet to see its headers at every layer.
There is no better way to internalize the layered model.

### ss — See Active Connections

```bash
# See all active connections and listening ports
ss -tuln
```

---

## Go Deeper

- **Computer Networking: A Top-Down Approach** (Kurose & Ross) — The standard textbook.
  Starts at the application layer and works down.
- **TCP/IP Illustrated, Vol. 1** (W. Richard Stevens) — The classic deep dive. Dense but
  authoritative.
- **High Performance Browser Networking** (Ilya Grigorik) — Free online. Networking from
  a web performance perspective: HTTP/2, QUIC, WebSockets, WebRTC.
- **Beej's Guide to Network Programming** — Free online. The classic intro to socket
  programming in C.
- **RFC 793 (TCP)**, **RFC 791 (IP)** — The actual specifications. Surprisingly readable.

---

*Next: [Distributed Systems](distributed-systems.md) — How do thousands of computers act as one?*

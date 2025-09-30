# Exercise sheet 2: BGP security (38 points) - *25 September 2025*

Handing in this exercise sheet is optional.
If you want individual feedback for your solutions, you have to hand in your solution by the **Wednesday following exercise publication, 01 October 2025, at 18:00**.
The hand-in procedure is as follows:

- copy this document, and answer the questions in the appropriate spaces;
- create a new issue on [the GitLab issues repo](https://gitlab.inf.ethz.ch/PRV-PERRIG/netsec-course/netsec-2025-issues);
- the issue title must be in the form `Exercise 02/BGP: {YOUR NETHZ ID}` (without curly braces);
- set the issue as _confidential_;
- the "Exercise Hand-In" label must be added to the issue;
- paste the modified document with your solution in the body of the issue.


### Question 1 (8 points)
**BGP Routing Attacks.**

**1.1.** (4 points)
> We have seen in the lecture that BGP is vulnerable to a multitude of attacks.
> 
> List two concrete attacks, with a very short explanation on how they work.
> For full points, also briefly explain their practical impact on society and mitigation stategies.

_Solution_:
First ISP hijacking such that some ISP tells the sender he's reaching out some other ISP so the traffic gets redirected to the second ISP. 
Second, a DDOS attack if some servers starts sending BGP packets to some AS boarder routers at a rate it is no more able to process. 

**1.2.** (4 points)
> There have been diverse types of research related to routing attacks.
> Several papers were introduced during the lecture.
> What is the difference in the approach of each paper? [1] [2]
> 
> Briefly describe each paper's attack strategy(vulnerability) and corresponding defense mechanism. 
> 
> [1] Bamboozling Certificate Authorities with BGP <https://secure-certificates.princeton.edu>
> 
> [2] RAPTOR: Routing Attacks on Privacy in Tor <https://www.usenix.org/node/190965>

_Solution_:
[1] 

1) A man-in-the-middle attack inorder to get bogus certificate from a trusted CA. 
Mitigation : Add multiple vantage CAs server for adding decentralization into the network. 

 2) A prefix IP hijacking to re route the victim's range of IP addresses.  
Mitigation : Authorizing lower range of IP address in the prefix to help identifying the source's address.

[2] 

1) The attack targets the TOR netowrk vulnerable at both ends of its flow. It uses the correlation between packets processing time and their size.
Mitigation : Identifying exit and guard TOR relays via frequency and time heurisitc for doing data encryption.  

### Question 2 (10 points)
**Securing BGP**

**2.1.** (2 points)
> Is BGP safe yet? Visit https://isbgpsafeyet.com/ (by CloudFlare) from your browser and test any ISP you have access to. This gets you one point.
> 
> For the second point, try to make the test succeed and paste a screenshot of a successful run. *Hint: perhaps another ISP could work, such as by using Cloudflare's free VPN?*

_Solution_:
(SWITCH, AS559)
ADD PIC

**2.2.** (3 points)
> Describe three design shortcomings of traditional BGP. What does BGP lack?
>
> 
> Focus on the security of routes and the availability guarantees of BGP, and list the currently proposed countermeasures, if any.

_Solution_:
First, it lacks security measure for identifying the request IP address. Second, it does not support congestion algorithm.  Third it has no understanding 
of a wider network topoloy for re-routing the request it receives. 

Countermeasures : 
1. Adding a signature for the client request and verifying if it actually matches a trusted certificate authority.
2. Can regulates the performance by inspecting the packet's size and the bandwidth available.
3. Can maintain a table mapping the routers of the AS it is discovering with a dietannce.

**2.3.** (3 points)
> What is ROA(route origin authorization)? Briefly explain how it works and
> against what attacks it is effective.

_Solution_:
A ROA states which AS is authorized to announce certain IP prefixes. It can sepcify the maximum IP prefiix IP prefix sime AS is authorized to advertise.
They are safe, distributed and checked out-of-band. It works as follow : first check if a ROA exists for the message, second check if it matches the AS entry of the message.

**2.4.** (2 points)
> Why is it not possible for a malicious AS to add another AS to a path in
> BGPSec?

_Solution_:
BGPsec prevents from path pretendiing of one AS to another AS because it must check the sgnature of the other ASes sittng next to it. 

### Question 3 (5 points)
**BGP: TCP RST Attack**

**3.1.** (2 points)
> BGP border routers use TCP sessions to reliably exchange routing
> advertisements, and this can become an attack surface to disrupt BGP
> routers. Specifically, an attacker sends the spoofed RST packet to close
> the TCP connection between two border routers, and the two routers
> delete advertisements learned from each other. At a later time, the two
> routers establish the TCP session again; however, there could be some
> delay after session establishment before any data traffic get passed on
> the link between the two routers.
> 
> What could have caused this delay, and how would you mitigate the TCP
> connection reset attack?

_Solution_:
After an attacker sends a TCP RST packet two routers would need to dscover each other back
through a TCP three way handhsake adding a delay. To mitigate a TCP connection reset attack, the 
AS needs to authenticate the routers.

**3.2.** (2 points)
> Imagine that in the previous example, the routers started using BGPSec.
> Unfortunately, admins haven’t fixed the vulnerability, so the adversary
> performs the same attack. Now the time between establishment of
> connections to live traffic jumps to several seconds. Explain why.

_Solution_:
In fact BGPsec adds latency for communication because it adds an additional layer of security mechanisms.

**3.3.** (1 points)
> When making forwarding decisions, BGP routers use longest prefix
> matching (LPM). For example, if you have both 1.1.0.0/16 and 1.1.1.0/24
> in your routing table, you will use the route indicated by the latter
> entry when sending the packet to 1.1.1.1.
> 
> In order to prevent route hijacking, every AS could announce the route
> for every IP address. Why is this a bad idea?

_Solution_:
It is not scalable if every AS annonce the route, because there are too many routes to be kept for each IP. 

### Question 4 (6 points)
**BGP: Recent attacks (1)**
On October 4th, 2021, Facebook experienced a global outage of its services,
internally as well as externally. We provide you with articles that should inform
you about the details of the event. In the following, you will answer questions
about the details of the outage. <br>
[Article from Cloudflare (external view, technical details)](https://blog.cloudflare.com/october-2021-facebook-outage/)
<br>
[Optional article from Facebook (internal view, high-level)](https://engineering.fb.com/2021/10/05/networking-traffic/outage-details/)
<br>

**4.1.** (2 points)
> What was the reason for the outage?

_Solution_:
This outage was triggered by the system that manages the global backbone network capacity.

**4.3.** (2 points)
> List any two ways an attacker could have abused this situation where Facebook
> BGP advertisements are gone.

_Solution_:
When Facebook BGPs advertisement were gone, an attacker could have advertsed Facebook prefixes
to another AS for DDOS. Or the attacker could have performed a man-in-the-mddle attack.

**4.4.** (2 points)
> Would these attacks also be possible with BGPsec deployed globally? Argue
> why or why not.

_Solution_:
BGPsec can protect from MITM attack as it uuses ROA keys so not everyone can advertse Facebook's routes.
DDOS is still possible with  BGPsec aware AS because it uses no signature to work. 

### Question 5 (9 points)
**Recent BGP Attacks (2)**

Prefix hijacks are the most common attacks in the BGP ecosystem. Attackers exploit 
by accident or by intention the longest matching prefix of BGP to redirect traffic through their AS. 
The list of attacks is pretty long, so we will highlight some of those in this exercise.

**5.1.** (4 points)
A recent BGP attack led to the theft of 1.9 millions worth of crypto from the South Korea platform KlaySwap[1].

[1] <https://medium.com/s2wblog/post-mortem-of-klayswap-incident-through-bgp-hijacking-en-3ed7e33de600>

**5.1.1.** (2 points)
> How did the attackers manage to serve malicious code to KLAYswap clients?

_Solution_:
Your solution here...

**5.1.2.** (2 points)
> Even if the attacker managed to redirect the clients from the legitimate KLAYswap server to its own,
> the download happened over HTTPS (this requires a valid TLS certificate).
> 
> How could the attacker have managed to obtain a certificate for KLAYswap?

_Solution_:
Your solution here...

**5.2.** (5 points)
KLAYswap is only one of the many incidents related to BPG hijacking. 
Read about this attack that happened on the 17th of August. [1]
Similarly to the KLAYswap case, the attackers managed to get a bogus certificate for `cbridge-prod2.celer.network`. 
Let's look at the BGP hijack that allowed this attack in more details.

[1] <https://arstechnica.com/information-technology/2022/09/how-3-hours-of-inaction-from-amazon-cost-cryptocurrency-holders-235000/>

**5.2.1.** (2 points)
> The attackers announced the ownership of a `/24` prefix, would the BGP hijack have been successful 
> if they announced the ownership of `44.192.0.0/10`?

_Solution_:
Your solution here...

**5.2.2.** (2 points)
> Amazon uses RPKI and had a ROA for the prefix that was hijacked, so why didn’t RPKI ROA help here?
> What trick did the attackers use to circumvent this defense measure?

_Solution_:
Your solution here...

**5.2.3.** (1 points)
> Suppose Amazon set a maximum prefix length to be `11` in the ROA. Would the bogus announcement
> have been blocked?

_Solution_:
Your solution here...

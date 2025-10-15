# Exercise sheet 04: Denial of Service Part 1 (30 points) - *09 October 2025*

Handing in this exercise sheet is optional.
If you want individual feedback for your solutions, you have to hand in your solution by the **Wednesday following exercise publication, 15 October 2025, at 18:00**.
The hand-in procedure is as follows:

- copy this document, and answer the questions in the appropriate spaces;
- create a new issue on [the GitLab issues repo](https://gitlab.inf.ethz.ch/PRV-PERRIG/netsec-course/netsec-2025-issues);
- the issue title must be in the form `Exercise 04/DoS/I: {YOUR NETHZ ID}` (without curly braces);
- set the issue as _confidential_;
- the "Exercise Hand-In" label must be added to the issue;
- paste the modified document with your solution in the body of the issue.


### Question 1 (7 points)
**DDoS: Main Attack Types.**

**1.1.** (3 points)
> Name the three main types of DDoS attacks. What part of the network
> infrastructure do each one attack, and in what units do we measure their
> severity?

_Solution_:
Three types of DDos attacks are : Volumetric attack, Protocol attack, Application-Layer attack.
The first attacks the network links, the second attacks network devices (networking stack) and the 
third attacks the applications (end system of the network). For each one of them the units of 
severity are respectively : bits per second, packet per second, request per second.

**1.2.** (2 points)
> Name some representative attack vectors for each of the 3 types of DDoS.

_Solution_:
For Volumetric attack a representative attack vector is UDP packet flood.
For Protocol attack a representative attack vector is TCP flood.
For Application-layer attack a representative attack vector is Database flood pool exhaustion.

**1.3.** (2 points)
> What kind of attacks are the most difficult to detect? Why do you think so?

_Solution_:
Application-Layer attacks are the most difficult to detect because the malicious requests look like legitimate user traffic, they are using correct headers, valid TLS, and normal-looking request patterns so volume or signature-based network detectors often miss them.

### Question 2 (4 points)
**Volumetric Attacks: Shrew Attack**

The lectures briefly mentioned Shrew Attacks as a potential attack vector
for volumetric DDoS attacks. Let's learn about it in a little bit more
depth.

Shrew Attacks can cause TCP traffic on a link to degrade significantly,
potentially even down to zero. But unlike typical attacks that try to fill
the bandwidth by sending high-rate attack traffic (and is thus easily
detected), shrew attacks cleverly sends packets at sufficiently low
average rate to elude detection by counter-DoS measures.

This information is sufficient to solve the questions below, but for those
who are curious, the paper below explains the mechanism in depth (and
may make it easier to answer questions):

[1] <https://dl.acm.org/doi/pdf/10.1145/863955.863966>

**2.1.** (2 points)
> Shrew attacks cleverly abuse TCP's resending mechanism to perform a
> DoS attack while maintaining low amounts of __average__ attack traffic.
> Can you think of how they might do this?

_Solution_:
They send very short, high-intensity bursts timed to cause loss across many TCP flows but keep the bursts infrequent enough that the average attack rate is low.
By aligning the burst period with TCP's retransmission-timeout behavior on many connections so the throughput collapses and never recovers between pulses while the attacker uses little average bandwidth.

**2.2.** (2 points)
> The diagram in the figure below provides parameters `l`, `R`, and `T`.
> Can you describe how an attacker would choose these values, with respect
> to the Round Trip Time of the link, the victim's Retransmission Timeout,
> and the link's bandwidth capacity?
> 
> <img src="assets/Square-wave_DoS_stream.png" alt="image" />

_Solution_:
Your solution here...

### Question 3 (12 points)
**DDoS: DNS.**

DDoS attacks on links are always very tricky to mitigate, and local
countermeasures are futile. This is because regardless of what you do with
the high amounts of incoming traffic (drop them, handle them, etc), your
outgoing connection to the internet is often also unavailable because you
use the same exhausted link there.

In this question we'll look at a specific type of attack that causes such
scenario: DNS reflection attacks.

On March 18, 2013 an anti-spam-mail organization, Spamhaus, experienced a
DDoS of bandwidth unheard of before then – 75 Gbps[1]. Skim through the
article below (especially the paragraph on "How to Generate a 75Gbps DDoS")
and answer the questions below:

<https://blog.cloudflare.com/the-ddos-that-knocked-spamhaus-offline-and-ho/>

[1] Note that more modern DDoS have reached several Tbps.

**3.1.** (1 points)
> How does a DNS reflection attack work?

_Solution_:
Your solution here...

**3.2.** (2 points)
> Can you give an example of a DNS query which would trigger a big response?

_Solution_:
Your solution here...

**3.3.** (1 points)
> In this case attackers probably used a request that is \~30 bytes long,
> which resulted in a \~3,000 bytes response. What is the amplification
> factor?

_Solution_:
Your solution here...

**3.4.** (3 points)
> Write down your local internet's speed, measured with https://fast.com/.
> (1 point)
> 
> Using the amplification factor calculated above, calculate the scale of
> DDoS attack (in Gbps) that you can cause on a victim DNS server.
> (2 points)

_Solution_:
Your solution here...

**3.5.** (3 points)
> DNS (mainly) uses UDP port 53.
> 
> Can you still perform a reflection attack with a TCP-based protocol? If
> you want, you can assume a Dolev-Yao attacker who can also
> intercept packets on the links between open DNS servers and the victim.

_Solution_:
Your solution here...

**3.6.** (2 points)
> What is an open recursive DNS resolver? How can you configure it to
> mitigate DNS reflection & amplification attacks?

_Solution_:
Your solution here...

### Question 4 (7 points)
Consider an attacker that wants to perform a DDoS attack on a victim server.
The attacker has created a botnet with 10,000,000 compromised IoT devices but
is _unable to spoof source addresses_.

**4.1.** (2 points)
> The attacker uses the botnet devices to directly flood the victim server.
> If each IoT bot device simply sends 10 TCP SYN packets per second
> towards the victim, what is the approximate aggregate bandwidth that
> reaches the victim? Reference the TCP header size, IPv4/6 header size,
> and give your answer in the unit of bits per second.

_Solution_:
Your solution here...

**4.2.** (3 points)
> Consider a scenario where the victim server uses an address filtering approach.
> Botnet nodes are blacklisted (based on their pattern of connection requests),
> and the associated packets are rejected.
> 
> Clearly, the advantage is that malicious SYN requests would be dropped.
> 
> But can you think of disadvantages or outstanding issues with this approach?

_Solution_:
Your solution here...

**4.4.** (2 points)
> Now assume that botnets are capable of spoofing addresses. Consider a
> service which publishes a list of misbehaving hosts, categorized as
> those performing 20 consecutive SYN requests to one of the servers in
> the network controlled by the service. Any server may periodically
> download the blacklist and refuse connections by listed hosts. Describe
> any two disadvantages of this approach.

_Solution_:
Your solution here...

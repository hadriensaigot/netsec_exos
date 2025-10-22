# Exercise sheet 05: Denial of Service Part 2 & Crypto Refresher (37 points) - *16 October 2025*

Handing in this exercise sheet is optional.
If you want individual feedback for your solutions, you have to hand in your solution by the **Wednesday following exercise publication, 22 October 2025, at 18:00**.
The hand-in procedure is as follows:

- copy this document, and answer the questions in the appropriate spaces;
- create a new issue on [the GitLab issues repo](https://gitlab.inf.ethz.ch/PRV-PERRIG/netsec-course/netsec-2025-issues);
- the issue title must be in the form `Exercise 05/DoS/II + crypto: {YOUR NETHZ ID}` (without curly braces);
- set the issue as _confidential_;
- the "Exercise Hand-In" label must be added to the issue;
- paste the modified document with your solution in the body of the issue.


### Question 1 (10 points)
Remotely Triggered Black Hole Filtering (**RTBH** in short) is a generic
technique that can be used to mitigate volumetric DoS attacks – the
offending traffic is simply dropped (black-holed) at the border routers
of an autonomous system (AS). RTBH comes in two flavours, source-based
and target-based.

This information is enough to solve the exercise questions below, but for
those who are curious, below is a link to the white paper.

[Remotely Triggered Black Hole Filtering White Paper.](https://www.cisco.com/c/dam/en_us/about/security/intelligence/blackhole.pdf)

**1.1.** (4 points)
In source-based RTBH, all the traffic from the attacker’s subnets is
dropped by the target’s ISP. (Note for clarification that the red
"dropped" icon in the diagram is labelled on the arrows going from PEs
to the Target).

PEs stand for Provider Edges (essentially just edge routers), which are
routers at the edge of ISP networks that connect to external
users/customers.

![Source-based RTBH<sup>1</sup>](assets/source_based.png)

**1.1.1.** (2 points)
> Are the ISP network (incl. links) and the target device protected from
> DoS? Why or why not?

_Solution_:
Your solution here...

**1.1.2.** (2 points)
> Why is this technique hard to implement, in the case of a DDoS attack?
> Can you imagine which unintended consequences it may have?

_Solution_:
Your solution here...

**1.2.** (6 points)
In destination-based RTBH, all the traffic to the target’s subnets is
dropped by the target’s ISP. This is a quicker solution than
source-based RTBH, but has its own problems.

![Destination-based RTBH<sup>1</sup>](assets/dest_based.png)

**1.2.1.** (2 points)
> What do you think are the problems with destination-based RTBH? Would you
> say that the identified problem undermines its effectiveness as a DoS
> mitigation strategy?
> (Note: this question originally said "source-based RTBH" as a typo; If you
> answered correctly for that version, you will still get the points.)

_Solution_:
Your solution here...

**1.2.2.** (3 points)
> Let us now delve into the technical details of destination-based RTBH. In our
> setup, the trigger router (manually configured by the admin) "installs"
> some configuration to the edge routers (PEs). How does this
> configuration work, and what does it change on the edge routers
> (explain to as much depth as you can)?
> 
> Although the diagram says "BGP updates" for the black arrow, you can
> assume iBGP (e.g. OSPF) is used within the ISP infrastructure.

_Solution_:
Your solution here...

**1.2.3.** (1 points)
> Normally, whenever a packet is dropped, an ICMP message would be sent
> back to inform the sender. Is this behaviour desirable here? Why?

_Solution_:
Your solution here...

### Question 2 (12 points)
(D)DoS attacks work by exhausting critical resources. For each attack:
1. identify the targeted critical resource;
2. discuss how the attack prevents the normal operation;
3. propose a mitigation.

**2.1.** (3 points)
> [Slowloris attack](https://www.incapsula.com/ddos/attack-glossary/slowloris.html)

_Solution_:
Your solution here...

**2.2.** (3 points)
> Flooding the embedded devices that use public key cryptography for
> message authentication with random messages.

_Solution_:
Your solution here...

**2.3.** (3 points)
> Sending packets crafted specifically to trigger the worst-case scenario in
> a hash-map.

_Solution_:
Your solution here...

**2.4.** (3 points)
> [TCP SYN flood attack](https://www.incapsula.com/ddos/attack-glossary/syn-flood.html): sending many TCP SYN requests to the victim
> server.

_Solution_:
Your solution here...

## Crypto refresher

### Question 1 (8 points)
On Thursday 16.10 you received a refresher lecture on cryptography. The following are some questions to help test your knowledge.
Concisely answer the following questions:

**1.1.** (2 points)
> Edward wants to prove to Laura he really is the sender of a message. What security property(s) is he trying to achieve?  Which cryptographic primitive could he use?

_Solution_:
Your solution here...

**1.2.** (2 points)
> Edward wants to send a secret message to Glenn---they cannot meet to exchange a key, but they deem it unlikely for the NSA to tamper with their messages on the fly ([1]). What security property(s) are Edward and Glenn trying to achieve? Which cryptographic primitives could they use?
> 
> [1]: Not historically accurate

_Solution_:
Your solution here...

**1.3.** (2 points)
> Edward wants to store records of his job assignments at NSA on his NAS. Which property(s) ensures that nobody will alter the record before it reaches the NAS? Can you name a network protocol which provides this guarantee? (assume a fair use of the protocol)

_Solution_:
Your solution here...

**1.4.** (2 points)
> Chelsea wants to share some documents with a journalist, without risking to be identified.  What security property is she trying to achieve?  Can you name a technology that could, in principle, protect her?

_Solution_:
Your solution here...

### Question 2 (2 points)

Challenge question (we did not reach this part during the crypto refresher session. If you are interested, we encourage you to read a little bit about 'Merkle Trees', as they might appear later in the course):

> Given a Merkle Hash Tree with n leaves, how many nodes need to be recomputed if a leaf is updated? (Assume a binary balanced fully populated tree).

_Solution_:
Your solution here...

### Question 3 (2 points)
> You are using AES in ECB mode to encrypt an uncompressed high-resolution image. What could go wrong?

_Solution_:
Your solution here...

### Question 4 (3 points)

Challenge Question:

You are writing a network protocol to control your server remotely---you decide to simply use AES in CBC mode to encrypt communication.
Unfortunately, CBC mode is susceptible to replay attacks and command manipulation, since modifying the ciphertext changes the plaintext in predictable ways.
You decide to add a MAC to prevent manipulation. Specifically, you add a MAC to all commands, and pad the combination of plaintext+MAC to a proper multiple of the block size before encrypting them. You might be using a padding scheme like PKCS7.
Finally, You also modify the server to now report an error if the padding is invalid, and a different error if the padding was valid but the MAC verification failed.

**4.1.** (3 points)
> What could still go wrong?

_Solution_:
Your solution here...
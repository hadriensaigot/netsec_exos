# Exercise sheet 03: SCION (34 points) - *02 October 2025*

Handing in this exercise sheet is optional.
If you want individual feedback for your solutions, you have to hand in your solution by the **Wednesday following exercise publication, 08 October 2025, at 18:00**.
The hand-in procedure is as follows:

- copy this document, and answer the questions in the appropriate spaces;
- create a new issue on [the GitLab issues repo](https://gitlab.inf.ethz.ch/PRV-PERRIG/netsec-course/netsec-2025-issues);
- the issue title must be in the form `Exercise 03/SCION: {YOUR NETHZ ID}` (without curly braces);
- set the issue as _confidential_;
- the "Exercise Hand-In" label must be added to the issue;
- paste the modified document with your solution in the body of the issue.


### Question 1 (4 points)
One of the principal aims of SCION is to provide users with transparency
and better control over the forwarding paths of network packets. Answer
concisely the following questions.

**1.1.** (2 points)
> In the context of routing architectures, what is the control plane? What
> does it do?

_Solution_:
The control plane constructs and disseminates the path along which a packet has to 
go in the network topology. 

**1.2.** (2 points)
> What is the data (or “forwarding”) plane? What does it do?

_Solution_:
The data plane combine the path information of a packet so that each router can 
correctly forward the data to the correct next hop.

### Question 2 (7 points)
**SCION vs IP**

**2.1.** (2 points)
> How is an IP packet forwarded when it reaches a border router? What
> happens instead in SCION? A sentence or two for each is enough.

_Solution_:
When it reached a border router the IP packet is inspected and given the IP 
prefix it contains, the router will send to the next router which annonces the
target IP prefix via heuristics (?). Instead in SCION, the path is already known so the router annonces the next hop the packet has to go to directly.

**2.2.** (2 points)
> How do those different routing architectures influence the size of a
> packet?

_Solution_:
The size of a packet is bigger in SCION because it contains the path segments that is used to build the entire path whereas an IP packet will certainly only contain the next destination IP.

**2.3.** (1 points)
> Consider this scenario: network failure interrupts an inter-AS link, and
> BGP eventually reconverges to start using a backup link. Besides the
> connectivity interruption, how is a host using that link affected by a
> change in the network topology? Will they be aware of it?
> 
> (Think about what information about the routing is available to the
> end-hosts.)

_Solution_:
The end-hosts does not know the ASes identities, so they will encounter a bigger latency while communicating because the ASes have to first re-discover themself before sending packet among each other. The end-hosts will not be aware of this as they use the same IPs addresses despite the network failure.

**2.4.** (2 points)
> Consider the same setting as above in SCION: what are the differences?
> Can you think of an advantage of this approach?

_Solution_:
In SCION all the paths between ASes are stored in the server of the AS communicating and because of the multi-path nature of SCION the end-hosts will not have extra work to do for the transmission. 

### Question 3 (6 points)
**SCION vs BGP**

**3.1.** (2 points)
> How are available paths determined in BGP? How does it compare to SCION?
> (A sentence or two for each is enough.)

_Solution_:
In BGP the server is advertising for a prefix range and the avaibility of the paths are determined by an economic factor. In SCION the paths are stored in a local server and the availability depends on many other factors like latency, bandwidth or carbon intensity.

**3.2.** (2 points)
> What happens when a link fails in BGP and SCION?

_Solution_:
In BGP if a link fails it will retry possibly on the same link but in SCION it will use another link.

**3.3.** (2 points)
> One key property of BGP is that ASes influence how traffic is routed
> based on BGP policies. In SCION, how do ASes control how packets are
> routed?

_Solution_:
In SCION the ASes control how packets are routed via the packet control plane.

### Question 4 (10 points)
In this exercise, we take a closer look at the control plane
implementation of SCION.

**4.1.** (2 points)
> What is an ISolation Domain (ISD)? What is a Core AS?

_Solution_:
An Isolation Domain is a sub-network consisting of multiple ASes that communicate frequently like a country. A core AS is the AS that is responsible for communicating with other core ASes in different ISD.

**4.2.** (4 points)
> Explain what is beaconing and how the distribution of PCBs (Path-segment
> Construction Beacon) works.

_Solution_:
Core ASes initiate Path-segment Construction Beacons, PCBs traverse ISD as a flood to reach downstream ASes, each AS receives path segments to a core AS.

**4.3.** (3 points)
> What is a path segment? Why is it called that way? What is the
> difference between up, down and core path segments?

_Solution_:
A path segment is a part of a path, it is called that way because it is smalled then a path and is used to build up a path. 
Down-path segment : from core AS to AS.
Up-path segment : from AS to core AS.
Core-path segment : between two core ASes.

**4.4.** (1 points)
> Each AS has its own path server. There are regular ASes and core ASes.
> After beaconing, what type of path segments do each of them store? (excluding caches at various levels)

_Solution_:
Up-path segments are registered at local path servers. Path servers provide a lookup service for segments to an AS. Down-path segment are uploaded to core path server in core AS after beaconing.

### Question 5 (7 points)
**About the Project**

In the SCION project that was just released, you are to implement a
SCION client application (end-host). We provide a "SCION daemon", which
gives you an API to retrieve available paths from your AS to a remote AS.
The remote AS contains the grading/verifier host. Your task is to send
packets along specifically chosen paths to the remote verifier.

Notably, the SCION daemon runs on your end host together with the client,
and is separate from (but talks to) your AS's path server. It's a sort of
local cache of path segments.

**5.1.** (1 points)
> Is a SCION daemon absolutely necessary for SCION to function, or do you
> think one can build an end host client application without using one?
> 
> If you can build a client without the daemon, why is a daemon still
> useful?

_Solution_:
Your solution here...

**5.2.** (2 points)
> In order to grade your choices (on which path you chose to send a packet
> along), the verifier host somehow knows about the path which a SCION
> packet traversed. How can they do that? Can client apps do the same?

_Solution_:
Your solution here...

**5.3.** (2 points)
> In one of the later questions, you will be asked to use FABRID to select
> your path to reach the remote verifier. FABRID is an extension to SCION,
> where ASes that have it enabled can choose to expose some of its
> forwarding policies for end hosts to use (alongside path segments).
> 
> For example, an AS may make available the policy: "If you want, we
> support forwarding only over devices produced by manufacturer A". Then,
> FABRID-enabled clients can use these as constraints when querying for
> paths, and may choose to apply them to packetes it sends out (e.g. by
> encoding information like "For this packet, at hop 2, apply policy L100;
> at hop 3, apply policy L200.")
> 
> What are some realistic settings where clients may want to use the
> additional flexibility of FABRID to select paths?

_Solution_:
Some realistic settings is for security purposes like apply this stronger encryption policy over this link because it is not secrued enough.

**5.4.** (2 points)
> One can argue FABRID exposes internal details of intermediary AS
> topologies, which could be bad for security. Why is this an ill-formed
> argument to make?

_Solution_:
It is a bad argument because FABRID only exposes the way of how a link should behave while transmitting a packet which tells nothing about the actual content of it.

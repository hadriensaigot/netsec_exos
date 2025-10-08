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
Your solution here...

**1.2.** (2 points)
> What is the data (or “forwarding”) plane? What does it do?

_Solution_:
Your solution here...

### Question 2 (7 points)
**SCION vs IP**

**2.1.** (2 points)
> How is an IP packet forwarded when it reaches a border router? What
> happens instead in SCION? A sentence or two for each is enough.

_Solution_:
Your solution here...

**2.2.** (2 points)
> How do those different routing architectures influence the size of a
> packet?

_Solution_:
Your solution here...

**2.3.** (1 points)
> Consider this scenario: network failure interrupts an inter-AS link, and
> BGP eventually reconverges to start using a backup link. Besides the
> connectivity interruption, how is a host using that link affected by a
> change in the network topology? Will they be aware of it?
> 
> (Think about what information about the routing is available to the
> end-hosts.)

_Solution_:
Your solution here...

**2.4.** (2 points)
> Consider the same setting as above in SCION: what are the differences?
> Can you think of an advantage of this approach?

_Solution_:
Your solution here...

### Question 3 (6 points)
**SCION vs BGP**

**3.1.** (2 points)
> How are available paths determined in BGP? How does it compare to SCION?
> (A sentence or two for each is enough.)

_Solution_:
Your solution here...

**3.2.** (2 points)
> What happens when a link fails in BGP and SCION?

_Solution_:
Your solution here...

**3.3.** (2 points)
> One key property of BGP is that ASes influence how traffic is routed
> based on BGP policies. In SCION, how do ASes control how packets are
> routed?

_Solution_:
Your solution here...

### Question 4 (10 points)
In this exercise, we take a closer look at the control plane
implementation of SCION.

**4.1.** (2 points)
> What is an ISolation Domain (ISD)? What is a Core AS?

_Solution_:
Your solution here...

**4.2.** (4 points)
> Explain what is beaconing and how the distribution of PCBs (Path-segment
> Construction Beacon) works.

_Solution_:
Your solution here...

**4.3.** (3 points)
> What is a path segment? Why is it called that way? What is the
> difference between up, down and core path segments?

_Solution_:
Your solution here...

**4.4.** (1 points)
> Each AS has its own path server. There are regular ASes and core ASes.
> After beaconing, what type of path segments do each of them store? (excluding caches at various levels)

_Solution_:
Your solution here...

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
Your solution here...

**5.4.** (2 points)
> One can argue FABRID exposes internal details of intermediary AS
> topologies, which could be bad for security. Why is this an ill-formed
> argument to make?

_Solution_:
Your solution here...

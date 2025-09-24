# Exercise sheet 1: Networks refresher - *18 September 2025*

Handing in this exercise sheet is optional.
If you want individual feedback for your solutions, you have to hand in your solution by the **Wednesday following exercise publication, 24 September 2025, at 18:00**.
The hand-in procedure is as follows:

- copy this document, and answer the questions in the appropriate spaces;
- create a new issue on [the GitLab issues repo](https://gitlab.inf.ethz.ch/PRV-PERRIG/netsec-course/netsec-2025-issues);
- the issue title must be in the form `Exercise refresher/exercise: {YOUR NETHZ ID}` (without curly braces);
- set the issue as _confidential_;
- the "Exercise Hand-In" label must be added to the issue;
- paste the modified document with your solution in the body of the issue.


## General

### Question 1 (0 points)
This exercise sheets are not graded, but they do come with a leaderboard! At the end of the course, we'll be awarding the top three students with some prizes :)  
The leaderboard will, by default, be pseudonymous: you are assigned a random ID used to track your progress. However, if you are after eternal glory, you can elect to let us use your real name on the leaderboard. Can we use your real name? (yes/no/please exclude me from the leaderboard)

## Network refresher

### Question 1 (9 points)
Concisely answer the following questions:

**1.1.** (2 points)
Explain what bandwidth allocation is, and what requirements we pose on a bandwidth allocation algorithm.

_Solution_:

The bandwidth allocation is a task for allocating bandwidth to the senders. The requirements is needs on a bandwidth allocation algorithm is that it must be fair (the sender gets a reasonable share of the network) and efficient which means that the full capaciity is used and there is no congestion.
 

**1.2.** (1 points)
How does UDP deal with bandwidth allocation?

_Solution_:

At the transport layer, UDP is a connectionless and best-effort protocol and it has no control over the congestion monitoring.

**1.3.** (1 points)
How does TCP deal with bandwidth allocation?


_Solution_:

TCP, on the other hand, uses built-in congestion control and flow control algorithms (like Reno, Cubic, or BBR) to dynamically adjust its sending rate.
This makes TCP flows share available bandwidth more fairly, while UDP traffic can monopolize or starve without extra controls.


**1.4.** (2 points)
The simpler nature of UDP is not necessarily a disadvantage. When is UDP preferred over TCP?

_Solution_:

UDP is preferred over TCP when the protocol needs to be fast and efficient with less overhead of going through some checks over some fields of the packet.  Indeed because there is no conncetion establishment prior to the transmission is it thus more suitable for real time application that prefer low latency over reliability. 

**1.5.** (3 points)
TCP is more complex than UDP. This complexity results in overheads, both in time and storage space. Let's focus on storage: the server has to store the state of each connection in its memory. Do you see a potential problem with this?

_Solution_:
Your solution here...
If the server has to store the state of each connection in its memory just like in TCP, the memory of the server can be filled rapidly each time a connection arrived.  

### Question 2 (3 points)
Explain why congestion is detrimental to networks.

_Solution_:
Your solution here...

Congestion s detrimental for networks because it adds latency, degrades the bandwidth and potentially packet loss if the number of packets connection overflow the server's memory . 

### Question 3 (10 points)
Routing algorithms are at the heart of routers. Open Shortest Path First (OSPF), is a link-state protocol used in many real world scenarios. In OSPF, every link has a specific cost and the goal is to find the best possible route. Here is a simple explanation of how it works:
<https://www.auvik.com/franklymsp/blog/ospf-protocol-explained/>.

**3.1.** (5 points)
Consider the following network. The square nodes are local-area networks (LANs), while the round nodes are routers. LANs can only receive and generate packets, they can't route incoming traffic. Fill in the routing tables below.

![OSPF network](assets/ospf-network.png)

R1:

| target | next hop |
| :----: | :------: |
|   N1   |   R3    |
|   N2   |   R3    |
|   N3   |   R2    |

R2:

| target | next hop |
| :----: | :------: |
|   N1   |   R1    |
|   N2   |   R3    |
|   N3   |   R4    |

R3:

| target | next hop |
| :----: | :------: |
|   N1   |   R2    |
|   N2   |   R5    |
|   N3   |   R2    |


R4:

| target | next hop |
| :----: | :------: |
|   N1   |   R2    |
|   N2   |    N2   |
|   N3   |   N3    |

R5:

| target | next hop |
| :----: | :------: |
|   N1   |   R3    |
|   N2   |   N2    |
|   N3   |   R4    |


N1:

| target | next hop |
| :----: | :------: |
|   N2   |   R3    |
|   N3   |   R1    |

N2:

| target | next hop |
| :----: | :------: |
|   N1   |   R3    |
|   N3   |   R4    |

_Solution_:
Your solution here...

**3.2.** (3 points)
You want to isolate the flows N1 $`\rightarrow`$ N3 and N1 $`\rightarrow`$ N2. Does this configuration satisfy this property? If not, how would you change the costs so that it does?

_Solution_:
Your solution here...

**3.3.** (2 points)
An external authority forces you to add a static route (with maximum priority) that sends any packet that reaches R5 to the link R5 $`\rightarrow`$ R4. Ignoring the longer routes and the unused links, what problems can arise?

_Solution_:
Your solution here...

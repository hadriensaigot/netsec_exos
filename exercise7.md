# Exercise sheet 07: TLS Part 2 (31 points) - *30 October 2025*

Handing in this exercise sheet is optional.
If you want individual feedback for your solutions, you have to hand in your solution by the **Wednesday following exercise publication, 05 November 2025, at 18:00**.
The hand-in procedure is as follows:

- copy this document, and answer the questions in the appropriate spaces;
- create a new issue on [the GitLab issues repo](None);
- the issue title must be in the form `Exercise 07/TLS/II: {YOUR NETHZ ID}` (without curly braces);
- set the issue as _confidential_;
- the "Exercise Hand-In" label must be added to the issue;
- paste the modified document with your solution in the body of the issue.


### Question 1 (4 points)
Consider TLS 1.3.

**1.1.** (2 points)
> We saw in the lecture that the TLS 1.3 Handshake always authenticates
> the server, but client authentication is left optional. Realistically,
> why do you think it was designed that way? (Why would it be infeasible,
> or useless to authenticate every user that connects to a web server?)

_Solution_:
It would be infeasible to authenticate all the clients at the server side 
because the server would need to store every authentication keys for 
each clients requesting access to the server. Thus it will require 
an enormous amount of memory. 

**1.2.** (2 points)
> Most of TLS 1.3's handshake is encrypted. Why is this a good thing? What
> kind of insecurities can derive from having a cleartext handshake like
> in TLS 1.2 and below?

_Solution_:
Most of TLS1.3 is encrypted so anyone tempering the link from a client
to the server will not be able to know the request for establlishing
a connection is happening which prevents the connection to be hijacked
by a third party. Insecurities from having a cleartext handshake is 
to have a clear ciphersuite which leaks information about the 
encryption mechanism such as the public key exchange algorithm
and the clientHello data.

### Question 2 (9 points)
Let's take a look at the handshake in more depth.

We want to avoid downgrade attacks where possible in the protocol, as
older versions have known vulnerabilities (see e.g. POODLE, Lucky 13, etc).
Downgrading all the way down to SSLv3 with RC4 selected as a stream cipher
can cause statistical analysis attacks as seen in last week's exercise too.

**2.1.** (2 points)
> An attacker can drop or modify packets in the network. Can they
> downgrade the negotiated TLS version in TLS 1.3?

_Solution_:
When requesting a connection over TLS1.3, the handshake mechanism 
is different from TLS1.2 such that when the handhsake is actually 
completed, the packets should respect the protocol both 
parties agree on. Indeed next packets are in both caes encrypted 
with a derived key from the handshake procedure.

**2.2.** (3 points)
> Some (buggy) legacy web servers used to close the connection when
> they received a packet that contains an unknown newer TLS version field.
> As a workaround, during the transition from TLS 1.1 to 1.2, many clients
> started implementing a fallback mechanism for backwards compatibility:
> if a connection was closed by the server, they would attempt a new TLS
> handshake, but with a lower TLS version number.
> 
> In TLS 1.2, how might attackers abuse this to perform downgrade attacks?

_Solution_:
In TLS1.2 a possible attack is to eventually temper with the handshake 
of someone connection to trigger a fallback mechanism at the server side 
in order for further packets transmission to use TLS1.1. In this manner 
the server would close the primary connection and further packets would 
use the lower version protocol.

**2.3.** (2 points)
> TLS 1.0, 1.1, 1.2 etc all have an authenticated transcript verification
> mechanism at the end of the handshake just like TLS 1.3. Then, why
> does that not protect against the downgrade attack described in the
> previous question?

_Solution_:
A downgrade attack on TLS1.2 is happening during the first packet exchange
during authentication but the verification using signatures is eventually happening at the last step
of the handshake which already uses the pre-defined version. In this manneer the signatures keys also 
encrypt the version of the tempered version at both sides of the exchange. 


**2.4.** (2 points)
> When clients and servers support both TLS 1.3 and older TLS versions,
> the TLS 1.3 record protocol can actually prevent downgrade attacks to
> older protocol versions. Investigate how that mechanism works, by
> searching RFC8446 for "downgrade protection mechanism" and briefly
> summarise how it works from the perspective of both the server and
> client.

_Solution_:
So to avoid TLS1.3 one can encrypt the version in the packet header with 
the exchanged keys and during the transmission of the ClientHello, 
append the ciphertext which the server can check.

### Question 3 (10 points)
Let's investigate potential replay attacks.

**3.1.** (3 points)
> You discover that your TLS library uses an insecure pseudo-random number
> generator for nonces in the handshake. This PRNG has a very short period
> — its output is repeated every 10<sup>5</sup> accesses. Propose a
> possible attack, assuming a Dolev-Yao adversary.
> 
> Assume that the PRNG is used to generate Diffie Hellman key shares is
> different, and is unaffected by the insecurity.
> 
> Think about the different TLS 1.3 handshake modes (Full DHE, PSK,
> PSK + DHE). Is your attack possible in all modes, or no? Why?

_Solution_:
Your solution here...

**3.2.** (2 points)
> TLS 1.3 PSK mode (0-RTT) handshake uses a pre-shared key for its
> encryption and does not have a key exchange. Thus, we concluded in the
> lectures that it does not have forward secrecy.
> 
> TLS 1.3 PSK (EC)DHE mode attempts to re-introduce forward secrecy, while
> still enabling the benefits of 0-RTT (i.e. send application data with
> the first message already -- `EarlyData`).
> 
> Does this mode have any security tradeoffs that the application layer
> should be aware of?

_Solution_:
Your solution here...

**3.3.** (5 points)
> TLS 1.3 PSK (EC)DHE mode is in fact what is used for TLS 1.3 0-RTT
> Resumption.
> 
> The PSK in this case is derived from a master secret shared between the
> two parties. A "NewSessionTicket" message, issued optionally by the
> server during the handshake, associates this PSK with a "ticket", which
> is often the PSK encrypted and authenticated with a key that only the
> server knows. In this case, the key is called the STEK (Session Ticket Encryption Key).
> 
> With that in mind, read this short GitLab issue thread on GnuTLS (a
> commonly used TLS implementation library).
> 
> https://gitlab.com/gnutls/gnutls/-/issues/1011
> 
> How can a passive MitM attacker exploit the GnuTLS bug? What are the
> consequences?
> 
> What about an active MitM attacker?

_Solution_:
Your solution here...

### Question 4 (8 points)
The modern web has long moved away from the one-server-per-webpage paradigm.
Today, content is often replicated across multiple servers, for example in
content-delivery networks (CDNs). At the same time, a single server (with a
single IP address) may host many virtual machines serving many different
webpages. The server therefore needs to hold the certificates for all the
websites it is hosting.

This, however, poses a problem for TLS: during the TLS handshake, the server
does not know which certificate to send to the client, as the HTTP header
containing the hostname (the name of the webpage being requested) is sent
only after the handshake.

**4.1.** (2 points)
> The common way to solve this problem is to use the server name
> indication (SNI) extension to TLS. This is an unencrypted field in the
> TLS header, specifying the hostname of the service being requested.
> 
> Does this solution have privacy drawbacks? If so, argue why and what the
> possible mitigating strategies are, considering the wider ecosystem of
> protocols you often need to establish a TLS connection.

_Solution_:
Your solution here...

**4.2.** (2 points)
> Can you think of another "lower-layer" solution that would allows us
> to address multiple VMs on a single physical server? Mention advantages
> and drawbacks.

_Solution_:
Your solution here...

**4.3.** (4 points)
> Another way to solve the problem of having different certificates for
> each webpage/service on a server, is to use one common certificate for
> everything.
> 
> At USENIX '21, Brinkmann, Dresen, Merget, Poddebniak, Müller, Somorovsky, Schwenk, and Schinzel presented "ALPACA", an attack that can exploit
> such configuration.
> 
> Read the short "Introduction" paragraph on the ALPACA website:
> https://alpaca-attack.com/ (For this question, the website is enough and
> you are not required to read the full paper. But we encourage you to do
> so if you are interested.)
> 
> To the best of your ability, explain how this attack works.

_Solution_:
Your solution here...
